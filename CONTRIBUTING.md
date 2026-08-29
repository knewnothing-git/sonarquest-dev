# Contributing to SonarQuest

SonarQuest is a unified code quality and software composition analysis
platform — an open alternative to SonarQube and Black Duck in one tool.

Contributions are welcome from anyone. You do not need to be invited.

> **Licence pending.** A licence file has not been added yet. Until it is,
> the terms under which contributions are accepted are undefined. If you
> plan a substantial contribution, open an issue first and we will settle
> this before you spend time.

---

## Ways to contribute

You do not have to write a scanner to be useful.

| | |
|---|---|
| **Rules** | Semgrep rules for languages or frameworks we cover badly |
| **Language support** | Package manifest parsers, language detection |
| **False positives** | Report them with a reproducing snippet. This is the most valuable bug report you can file. |
| **Integrations** | GitLab, Bitbucket, Jenkins, Azure DevOps |
| **Frontend** | The dashboard has a lot of room |
| **Docs** | Setup guides, deployment walkthroughs, rule documentation |
| **Testing** | Run it on your own codebase and tell us what broke |

---

## Development setup

**Prerequisites:** Docker and Docker Compose, Python 3.12+, Node 20+, Git

```bash
git clone https://github.com/knewnothing-git/sonarquest-dev.git
cd sonarquest-dev

cp .env.example .env
make install          # backend + frontend dependencies
make up               # postgres + redis
make migrate          # database schema
make test             # confirm a clean baseline
```

Backend runs at `localhost:8000`, frontend at `localhost:5173`.

```bash
make check            # lint + typecheck + tests, run this before pushing
make down             # stop services
```

---

## Project structure

```
backend/
  app/
    api/          FastAPI routes (/api/v1)
    models/       SQLAlchemy models
    scanners/     Semgrep, Syft, Grype, OSV, ScanCode wrappers
    core/         fingerprinting, baseline diff, quality gates
    tasks/        Celery orchestration
  tests/
  migrations/     Alembic
frontend/
  src/
graph/            build DAG and status (see below)
docs/
```

---

## The build graph

This repo is built by Claude Code working through a task DAG. You will
see `graph/nodes.yaml` and `graph/state.json` and they may look unusual.

- `nodes.yaml` defines build units. Each has a `verify` that is a shell
  command returning 0 or non-zero.
- `state.json` tracks which are done.

**As a human contributor you can ignore both.** Do not edit them in a PR
unless your change is specifically to the build system. Normal
contributions — a rule, a bug fix, a frontend change — touch only the
usual directories.

---

## Adding a scanner

Every scanner normalizes to the shared `Finding` or `Component` model
before anything is stored. Scanner-specific output never reaches the
database.

1. Add `backend/app/scanners/<name>.py` implementing the `Scanner`
   protocol: `run(workspace: Path) -> list[Finding] | list[Component]`
2. Map its output to the shared model. Do not add fields to the model to
   accommodate one scanner — normalize instead.
3. Add a fixture in `backend/tests/fixtures/` containing a known finding
   the scanner must detect.
4. Add `backend/tests/test_<name>.py` with both a positive case (finds
   the planted issue) and a negative case (clean code produces nothing).
5. Register it in the orchestration chain.

---

## Adding language support

1. Extend detection in `backend/app/scanners/ingest.py`
2. Add the package manifest parser (`go.mod`, `Cargo.toml`, `pom.xml`)
3. Map packages to Package URLs (purl) so vulnerability matching works
4. Add a fixture project in that language

---

## Code standards

**Python** — 3.12, type hints everywhere, `ruff` and `mypy` clean.
**TypeScript** — strict mode, no `any`, `eslint` clean.

Two rules specific to this project:

**Fingerprints never use line numbers.** Findings are identified by
`(rule_id, normalized_snippet_hash, enclosing_symbol)`. A fingerprint
that changes when a file is reformatted breaks every user's suppressions.
Any PR that introduces a line-number dependency in fingerprinting will be
rejected.

**No secrets in code.** Configuration comes from environment variables.
No exceptions, including in tests.

---

## Testing

```bash
make test                                  # everything
pytest backend/tests/test_sast.py -q       # one file
cd frontend && npm run test                # frontend
```

Tests ship in the same commit as the code they cover. A PR that adds
behaviour without tests will be asked for tests before review.

For false-positive fixes, add the code that was wrongly flagged as a
negative test case. That is how the regression stays fixed.

---

## Pull requests

1. Fork, then branch: `feat/short-description` or `fix/short-description`
2. Make the change. Keep it focused — one concern per PR.
3. `make check` must pass.
4. Commit messages: `<area>: <what changed>`, e.g.
   `scanners: add Ruby gemfile.lock parser`
5. Open the PR against `main` and fill in the template.

**What gets a PR merged faster:**

- It does one thing
- It has tests
- It does not reformat files it did not otherwise change
- The description says what problem it solves, not just what it does

**What slows a PR down:**

- Drive-by refactoring of unrelated code
- New dependencies without a reason in the description
- Reformatting the whole file around a three-line change

---

## Reporting bugs

Open an issue with: what you ran, what happened, what you expected, and
your environment.

**For false positives**, include the smallest code snippet that triggers
it and the rule id. This is the single most useful bug report for this
project — precision is what makes or breaks a scanner in real use.

**For security vulnerabilities in SonarQuest itself**, do not open a
public issue. Open a GitHub security advisory on the repository instead.

---

## Good first contributions

- Add a Semgrep rule for a framework you know well
- Add a package manifest parser for a language we do not cover
- Improve an error message you found confusing during setup
- Write the deployment guide for your platform
- Report a false positive with a reproducing case

---

## Questions

Open a GitHub Discussion or an issue. Asking whether something is worth
building before you build it is always welcome, and usually saves both
sides time.
