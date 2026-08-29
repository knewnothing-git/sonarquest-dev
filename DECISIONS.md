# Decision Log

Append-only. Each entry records what was decided, the reasoning, and what
would reverse it.

---

## D-001 — Reject horizontal code-quality product

**Date:** 2026-08-29 · **Status:** Decided

Rejected building a general-purpose SonarQube alternative competing on
price and UX.

**Reasoning:** No compliance driver, no moat, and four funded competitors
already in the lane (DeepSource, Codacy, CodeAnt AI, Qodana). Pricing
disruption alone is a race to the bottom.

**Reverses if:** Never. This option is closed.

---

## D-002 — Reject vertical: AI-generated code governance

**Date:** 2026-08-29 · **Status:** Decided

**Reasoning:** Fastest-moving and most crowded segment. CodeRabbit,
Greptile, Panto, DeepSource, and Sonar all active. Low technical barrier
means differentiation collapses quickly. No founder edge.

---

## D-003 — Select vertical: automotive safety-critical embedded

**Date:** 2026-08-29 · **Status:** Decided, pending Phase 0 validation

Target MISRA C/C++, AUTOSAR, and ISO 26262 compliance for automotive
embedded software.

**Reasoning:**
- Demand is contractual, not discretionary. ISO 26262 is functionally
  mandatory in practice — an OEM gate for program entry and a
  prerequisite for European and Asian export markets.
- Incumbents (LDRA, Helix QAC, Parasoft, Polyspace) are 20–30 year old
  architectures with quote-based enterprise pricing and no AI layer.
- Founder has 10+ years automotive quality and precision metrology
  background, plus direct PPAP experience. Speaks the Quality Manager's
  and Functional Safety Manager's language, not just the developer's.
- Pune is the densest automotive software cluster in India.
- No new-generation entrant in this specific lane.

**Reverses if:** Phase 0 returns fewer than 8 of 15 calls scoring 4+ on
the pain thesis. See `docs/04-phase-0.md`.

---

## D-004 — Core insight: build the evidence layer, not a better scanner

**Date:** 2026-08-29 · **Status:** Decided

MISRA compliance is not "find violations and fix them." Per MISRA
Compliance:2020 it is a documented evidence process: Guideline
Enforcement Plan, Guideline Re-categorization Plan, deviation records
with written justification, and a Guideline Compliance Summary.

The bulk of the work is triage, deviation clustering, rationale writing,
approval, and audit-surviving documentation. Incumbents are strong at
detection and weak here.

**Consequence:** Do not compete on rule coverage in year one. Compete on
the compliance workflow. Be honest about coverage — automotive engineers
respect precision over marketing claims.

---

## D-005 — Evaluate and reject EU Cyber Resilience Act as the near-term wedge

**Date:** 2026-08-29 · **Status:** Rejected after competitive research

CRA (Regulation EU 2024/2847) was evaluated as a faster-to-revenue play
targeting embedded and IoT manufacturers. Attractive on paper: reporting
obligations from 11 Sept 2026, full application 11 Dec 2027, penalties up
to EUR 15M or 2.5% of global turnover, and a product-centric shape that
company-centric GRC platforms fit poorly.

**Rejected because the space is already occupied:**

| Player | Position |
|---|---|
| ONEKEY (Düsseldorf) | Product Cybersecurity & Compliance Platform. Compliance Wizard covers CRA, IEC 62443-4-2, ETSI EN 303 645, UNECE R155. Binary analysis without source. "CRA Fast Start" launched Feb 2026. PwC Germany portfolio. |
| Finite State (Ohio) | $62.5M raised since 2017. Unifies firmware, binaries, source and compliance evidence. Exports audit-ready evidence for EU CRA, FDA 524B, CE RED. |
| NetRise | Binary-analysis supply chain security. **Acquired by Accenture** — closes the consulting channel. |
| Binarly | $14.1M, firmware security, ML-based introspection. |

Also: SBOM generation is commoditized — Syft + Grype produce a CycloneDX
SBOM in a CI pipeline in under 90 seconds, free.

**What this validates:** ONEKEY and Finite State prove the business model
shape works — continuous analysis feeding a compliance evidence workflow
producing an audit-ready technical file. They proved it in a regime that
**structurally excludes automotive**: the CRA exempts vehicles, medical
devices, and aviation, which have their own regimes.

**Net effect:** strengthens D-003. Someone demonstrated the model works,
and there is no equivalent player in the MISRA / ISO 26262 lane.

---

## D-006 — GRC platforms are not the competitor set

**Date:** 2026-08-29 · **Status:** Noted

Vanta, Drata, Sprinto, Secureframe and Scrut share one architecture: they
connect to structured APIs and read configuration data. They do not read
code. Their European framework expansion covered EU AI Act, DORA and
NIS 2 — not CRA.

Analytical error worth recording: the first competitive pass checked GRC
platforms and concluded the space was open. The actual competitors were
product security platforms, which had been building since 2017. Check the
right category before concluding a market is empty.

---

## D-007 — Watch item: ONEKEY as future competitor

**Date:** 2026-08-29 · **Status:** Monitoring

ONEKEY already covers UNECE R155 (automotive cybersecurity, ISO 21434
territory). Extending into ISO 26262 functional safety and MISRA coding
standards is their natural expansion path.

Barrier: that requires deep C/C++ static analysis, a materially different
engineering discipline from binary scanning. Estimated 2–4 years before
credible entry, if ever.

**Action:** Re-check quarterly. Trigger for re-evaluation is any ONEKEY
or Finite State announcement touching functional safety or coding
standards.

---

## D-008 — Playbook to copy from ONEKEY

**Date:** 2026-08-29 · **Status:** Adopted

Their go-to-market motion is directly transferable:
- Annual industry readiness report → manufactures the urgency, then sells
  the cure
- Assessment-first offering → services revenue funds platform development
- Trade show presence where buyers actually are (ours: ARAI, SIAT,
  Automotive Testing Expo India, Embedded World)
- Platform plus consulting hybrid
- Strategic investor for credibility (theirs: PwC; ours would be a TÜV,
  a notified body, or an Indian ESP)

---

## D-009 — Replace the C++ engine spike with a thin Python prototype

**Date:** 2026-08-29 · **Status:** Decided · **Supersedes:** part of D-003 execution plan

The v1 graph had a `spike` track: build a Clang/LLVM MISRA analyzer from
scratch, implement 10 rules, measure false-positive rate.

**Rejected because the decision maker cannot verify it.**

Yogesh's background is automotive quality engineering, not compiler
engineering. A track whose outputs are AST matchers and precision
intervals gives him nothing he can independently judge — he would be
approving work on trust, which is how non-technical solo founders lose
control of their own product.

Worse, it left his actual expertise unused. He has ten years of PPAP and
audit documentation experience. The question "would an OEM assessor
reject this compliance summary" is one almost nobody can answer well, and
he is one of them.

**Replacement:** a thin Python prototype that wraps Cppcheck's MISRA
addon for detection and builds only the evidence layer — deviation
register, GRP, and Guideline Compliance Summary generation.

| | C++ engine spike | Thin prototype |
|---|---|---|
| Language | C++ / Clang LibTooling | Python |
| Detection | Built from scratch | Existing OSS addon |
| Terminal artifact | A precision figure | A GCS document |
| Who judges it | The agent that built it | Yogesh, from his own audit experience |
| Time to demoable | 8–12 weeks | 2–3 weeks |

**It also tests the actual thesis.** D-004 says the compliance workflow is
the pain, not the detection. Wrapping a detector and building only the
workflow is the direct test of that claim. Building a detector first
tested something the thesis does not depend on.

**Known constraint:** Cppcheck's MISRA addon is GPL-3.0. The prototype is
therefore validation and demo only, never shippable. Replacing the
detector is node PR-02, post-Phase-0, funded, and probably not solo.

**Reverses if:** Phase 0 confirms the thesis, revenue exists, and the
detector becomes the binding constraint. At that point the C++ work
returns as a staffed project rather than a solo one.

---

## D-010 — Operating model: driver mode, autonomy bounded by verifiability

**Date:** 2026-08-29 · **Status:** Decided

Governing principle for all agent work on this project:

> Agent autonomy on any task is limited by Yogesh's ability to verify the
> output. Where he can check the result, agents work and report. Where he
> cannot, they stop and surface the decision in terms he can judge.

**Mechanisms:**

- `mode: driver` in `graph/state.json` — one step, then stop. Autopilot
  is granted per-node, out loud, and never carries across nodes.
- `he_verifies: true` on any node whose completion is a professional
  judgment call. Agents present evidence and stop; they never mark these
  done.
- `_owner: yogesh` on state fields agents may not write —
  `phase0.decision`, `prototype.corpus`, `prototype.gcs_verdict`.
- `/brief` command — translates project state into plain language and
  names the decisions waiting on him. Session entry point.
- Agents are instructed to report artifacts, not code, and to convert
  metrics into questions answerable from domain experience.

**Why this matters more than usual here:** the failure mode for a
non-technical founder running agentic workflows is silent. Bad work looks
identical to good work in a diff you cannot read, and the cost surfaces
months later. Structural constraints are the only reliable defence;
intending to review carefully is not one.

**Reverses if:** never in spirit. The specific mechanisms can change as
his familiarity with the codebase grows — a node that needs approval
today may not in a year.
