# Business Model

Open core. The engine is free and AGPL-licensed; the things teams need at
scale are paid.

**This file exists so features do not land in the open repo by accident.**
Before building anything, check which column it belongs in.

---

## Why AGPL-3.0

| | |
|---|---|
| **Distribution** | Free self-hosting is the only channel available without a brand, a sales team, or a marketing budget. SonarQube reached millions of installs this way before upselling. |
| **Defence** | AGPL requires anyone running a modified version as a network service to publish their source. A competitor cannot take this and launch a hosted SonarQuest without opening their changes. Apache-2.0 would let them. |
| **Revenue mechanism** | Large enterprises routinely prohibit AGPL in their dependency policy. "Our legal team will not approve AGPL" is a sales conversation, not a lost deal. The commercial licence is the answer. |

**The CLA is load-bearing.** Without copyright grants from contributors,
their code stays AGPL-only forever and cannot be included in a commercial
licence. That would kill the model. See `CLA.md`.

---

## Free (AGPL, self-hosted)

Everything needed to prove the tool works on a real codebase:

- SAST via OpenGrep — full language coverage
- SCA: dependency inventory, CVE matching, license identification
- SBOM export (CycloneDX, SPDX)
- Quality gates on the default branch
- CLI scanner for any CI system
- Single-project dashboard
- Community rules

**Deliberately generous.** A crippled free tier gets abandoned before it
ever demonstrates value. This must be genuinely useful on its own.

---

## Paid

The line follows SonarQube's proven split: single-branch analysis is
free, everything a *team* needs is paid.

**Team tier**
- Pull request and merge request decoration
- Branch and feature-branch analysis
- Multi-project portfolio view
- SSO (SAML, OIDC)
- Role-based access control
- Historical trends beyond 30 days

**Enterprise tier**
- Policy management across projects
- Audit logs
- SBOM attestation and signing
- Custom rule authoring UI
- Priority support and SLA
- Air-gapped deployment support

**Hosted SaaS**
- Everything above with zero deployment work
- Per-repository or per-developer pricing
- The lowest-friction path to first revenue, and where a solo founder
  should focus

**Commercial licence**
- For organisations that cannot use AGPL
- Priced per organisation, not per seat

---

## Pricing

Deliberately unset. Anchor against current SonarQube and Snyk list
pricing when you get there, and undercut on the per-developer SaaS tier —
that is where both incumbents are weakest for teams under 200 engineers.

Validate with real buyers before publishing numbers.

---

## Repository split

Paid features do not live in this repository.

```
sonarquest-dev        public, AGPL-3.0   — engine, scanners, core, CLI, dashboard
sonarquest-enterprise private            — SSO, RBAC, PR decoration, policy, audit
sonarquest-cloud      private            — hosted SaaS, billing, tenancy
```

The private repos consume the public one as a dependency. Never invert
that — an AGPL core importing proprietary code is a licence violation.

---

## Upstream licence constraints

The scanners this product wraps have their own terms. These are hard
constraints on what can ship.

| Tool | Licence | Constraint |
|---|---|---|
| **OpenGrep** | LGPL-2.1 | Execute as a subprocess. Never link or vendor the binary. |
| **Syft** | Apache-2.0 | No constraint |
| **Grype** | Apache-2.0 | No constraint |
| **OSV** | Open data | No constraint |
| **ScanCode** | Apache-2.0 | No constraint |

**Semgrep is excluded.** Its engine is LGPL-2.1, but its maintained rules
moved to the Semgrep Rules License v1.0 in December 2024 — internal,
non-competing, non-SaaS use only. A commercial SAST platform is precisely
what that excludes. OpenGrep is the LGPL-2.1 fork built for this
situation, governed by a consortium of appsec vendors rather than a
single competitor.

**Rule sources must be checked individually.** Registry rules inherit the
licence of their source repository — some are AGPL-3.0, some are
proprietary. Ship only rules that are OSS-licensed or written in-house.
