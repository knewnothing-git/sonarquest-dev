# SonarQuest — Autonomous Build Context

Unified code quality + software composition analysis platform.
Open alternative to SonarQube (SAST, quality gates) and Black Duck
(SCA, SBOM, license compliance).

**You run this build autonomously. Decide, build, verify, commit, continue.**

---

## Operating mode: autonomous

Read `graph/state.json`, pick the next unblocked node, build it, run its
`verify` command, commit on green, move to the next. Do not stop to ask
permission between nodes.

**Every node's `verify` is a shell command that returns 0 or non-zero.**
That is what makes autonomy safe here — completion is machine-checked,
not judged. If you find yourself wanting a human opinion to close a node,
the node's `verify` is wrong. Fix the verify, don't escalate.

Only stop for: a failed node after 3 repair attempts, a missing
credential, or an external service you cannot reach.

---

## Decision policies — apply these, do not ask

| Situation | Policy |
|---|---|
| Spec is ambiguous | Pick the simplest reading that satisfies `verify`. Log it in `DECISIONS.md`. Continue. |
| Two valid designs | Pick the one with fewer moving parts. Log it. Continue. |
| `verify` fails | Fix and retry, up to 3 attempts. On the 4th, mark the node `blocked` with the error text and move to the next unblocked node. |
| Scope creep | Never expand a node. Extra ideas go to `BACKLOG.md`. |
| New dependency | Allowed if it is on the approved list below. Otherwise add it, log why in `DECISIONS.md`, continue. |
| Missing test fixture | Create it. Fixtures live in `tests/fixtures/`. |
| Something looks wrong in an earlier node | Log it in `BACKLOG.md`. Do not refactor it now. |
| Secret or credential needed | Stop. This is the only hard stop. |

---

## Stack — locked, do not revisit

```
Backend       Python 3.12, FastAPI, SQLAlchemy 2.x, Alembic
Worker        Celery + Redis
Database      PostgreSQL 16
Frontend      React 18 + Vite + TypeScript + Tailwind
Scanners      OpenGrep (SAST), Syft (SBOM), Grype + OSV (vulns), ScanCode (licenses)
Packaging     Docker, docker-compose
Tests         pytest, vitest
Lint          ruff, mypy, eslint
```

Approved dependency list: anything in the stack above, plus `httpx`,
`pydantic`, `structlog`, `typer`, `gitpython`, `packageurl-python`,
`cyclonedx-python-lib`.

---

## What the product does

**SAST side (SonarQube parity)**
Multi-language static analysis, bugs and security findings, quality
gates that pass/fail CI, baseline and new-code-only enforcement, PR
decoration, per-project dashboards, trend history.

**SCA side (Black Duck parity)**
Dependency inventory, transitive resolution, CVE matching against OSV,
license identification and policy enforcement, SBOM export in CycloneDX
and SPDX, supply chain risk scoring.

**The combination is the wedge.** SonarQube does not do SCA well, Black
Duck does not do SAST. One platform, one gate, one dashboard.

---

## Architecture

```
GitHub/GitLab webhook
        v
   API (FastAPI)  ->  Postgres
        v
   Celery queue (Redis)
        v
   Scanner worker (Docker)
     ├─ OpenGrep     -> code findings
     ├─ Syft         -> SBOM components
     ├─ Grype/OSV    -> vulnerabilities
     └─ ScanCode     -> licenses
        v
   Normalizer -> fingerprints -> Postgres
        v
   Quality gate engine -> pass/fail -> PR status
        v
   React dashboard
```

---

## Conventions

- Every module gets tests in the same commit. No exceptions.
- Findings are fingerprinted on `(rule_id, normalized_snippet_hash,
  enclosing_symbol)`. **Never on line number** — a fingerprint that moves
  on reformat breaks every suppression downstream.
- All scanner output normalizes to one `Finding` model before storage.
  Scanner-specific shapes never reach the database.
- API is versioned under `/api/v1`. OpenAPI schema must stay valid.
- Migrations are Alembic. Never edit a shipped migration.
- Config from environment only. No secrets in code, ever.
- Commit per node: `<node-id>: <what changed>`.

---

## Files

```
graph/nodes.yaml     build DAG, each node with a runnable verify command
graph/state.json     node status — you update this as you go
DECISIONS.md         append-only log of choices you made autonomously
BACKLOG.md           deferred ideas and problems found in passing
docs/archive/        earlier MISRA vertical strategy, retained for reference
```

Commands: `/run` (autonomous loop), `/next` (one node), `/status`,
`/ship` (deploy).

---

## Definition of done for a node

1. `verify` command exits 0
2. `ruff check` and `mypy` clean on changed files
3. Tests written and passing
4. Committed with the node id in the message
5. `graph/state.json` updated

Then take the next node without asking.
