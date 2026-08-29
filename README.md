# SonarQuest

Compliance evidence platform for safety-critical embedded software.

**Status:** Pre-validation. No code yet. Phase 0 (market validation) not started.

---

## What this is

Static analysis tools find MISRA violations. They are weak at everything
that happens next: triaging thousands of findings, clustering them into
deviation use-cases, writing justification rationales, getting them
approved, and producing an auditable Guideline Compliance Summary that
survives an OEM audit.

That downstream workflow is where the engineer-time and the anxiety
actually live. SonarQuest targets that gap.

**Positioning:** *"Helix QAC finds your violations. SonarQuest gets you
through the audit."*

**Not** a SonarQube competitor. The competitors are LDRA, Perforce Helix
QAC, Parasoft, and MathWorks Polyspace.

---

## Documents

| File | Contents |
|---|---|
| [DECISIONS.md](DECISIONS.md) | Decision log — what was chosen, what was rejected, why |
| [docs/01-strategy.md](docs/01-strategy.md) | Vertical selection, thesis, competitive landscape |
| [docs/02-product.md](docs/02-product.md) | Product definition and technical architecture |
| [docs/03-gtm.md](docs/03-gtm.md) | Pricing, go-to-market, roadmap, risks |
| [docs/04-phase-0.md](docs/04-phase-0.md) | Validation kit — call script, scoring, target list |

---

## Immediate next step

Phase 0 validation. 15 discovery calls over 4 weeks, plus an IP counsel
consult on MISRA text licensing. No code until the thesis is confirmed.

Go/no-go threshold and kill criteria are in `docs/04-phase-0.md`.

---

## Open items

- `.gitignor` in repo root is misnamed (missing `e`) — currently inert
- Stale `master` branch exists with a divergent initial commit
- Project name "SonarQuest" anchors to SonarQube, which is the wrong
  frame for this positioning — rename under consideration
