# SonarQuest

Unified code quality and software composition analysis. Open alternative
to SonarQube (SAST, quality gates) and Black Duck (SCA, SBOM, license
compliance).

**Status:** In build. See `/status`.

---

## What it does

**Code analysis** — multi-language SAST via OpenGrep, security and quality
findings, fingerprinted so suppressions survive refactoring, baseline
diffing so you can enforce on new code only, quality gates that fail CI.

**Dependency analysis** — full component inventory including transitive
deps, CVE matching against Grype and OSV, SPDX license identification
with policy enforcement, SBOM export in CycloneDX and SPDX.

One platform, one gate, one dashboard. SonarQube does not do SCA well;
Black Duck does not do SAST. That gap is the product.

---

## Stack

Python 3.12 / FastAPI / Celery / PostgreSQL / Redis · React + Vite +
TypeScript + Tailwind · OpenGrep, Syft, Grype, OSV, ScanCode · Docker

---

## Build system

This repo builds itself through Claude Code.

```
CLAUDE.md          product spec + autonomous decision policies
graph/nodes.yaml   18-node build DAG, each with a runnable verify command
graph/state.json   live status
DECISIONS.md       every autonomous choice, logged
BACKLOG.md         deferred ideas
```

**Commands**

| | |
|---|---|
| `/run` | Build autonomously until blocked or complete |
| `/run F-05` | Build one node |
| `/next` | Build the next node, then stop |
| `/status` | Progress and blockers |

Every node's completion is a shell command that exits 0 or non-zero.
Nothing is "done" by judgment.

---

## Getting started

```bash
make install
make up
/run
```

---

## Phases

| Phase | Nodes | Scope |
|---|---|---|
| 1 Foundation | F-01…03 | Scaffold, API skeleton, schema |
| 2 Scanners | F-04…08 | Ingest, SAST, SBOM, vulns, licenses |
| 3 Core | F-09…11 | Fingerprinting, gates, orchestration |
| 4 Surfaces | F-12…16 | API, CLI, dashboard, GitHub App, auth |
| 5 Ship | F-17…18 | Production compose, E2E smoke |

---

## docs/archive

Earlier strategy work on a MISRA / ISO 26262 automotive vertical.
Retained as reference — not the current direction.

## Contributing

Contributions welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) for setup,
project structure, and how to add a scanner or language.

The most useful thing you can send us is a false positive report with a
reproducing snippet.


## Licence

**AGPL-3.0.** Free to self-host, modify and use. If you run a modified
version as a network service, you must publish your changes.

A commercial licence is available for organisations that cannot use AGPL.
See [docs/BUSINESS-MODEL.md](docs/BUSINESS-MODEL.md).

Contributors: see [CLA.md](CLA.md).