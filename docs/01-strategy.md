# Strategy

## 1. Vertical selection framework

A vertical wedge is worth taking only if it scores on all five:

| Criterion | Why |
|---|---|
| Mandatory demand | Compliance budget beats discretionary budget |
| Incumbent overpricing | Need a 5–10x price gap to get meetings |
| Domain moat | Rules encoding domain expertise resist copying |
| Founder edge | Can *this* founder win vs. a funded team |
| Reachable buyer | Can a solo founder in Pune get in the room |

## 2. Candidates scored

| Vertical | Mandatory | Overpriced | Moat | Edge | Reach |
|---|---|---|---|---|---|
| **Automotive (MISRA/ISO 26262)** | ★★★★★ | ★★★★★ | ★★★★★ | ★★★★★ | ★★★★★ |
| India BFSI (RBI/SEBI CSCRF) | ★★★★☆ | ★★★☆☆ | ★★★☆☆ | ★★★☆☆ | ★★★☆☆ |
| Healthcare (IEC 62304/HIPAA) | ★★★★☆ | ★★★☆☆ | ★★★☆☆ | ★★☆☆☆ | ★★☆☆☆ |
| AI code governance | ★★☆☆☆ | ★☆☆☆☆ | ★☆☆☆☆ | ★★★☆☆ | ★★★★☆ |
| Legacy enterprise (COBOL/ABAP) | ★★★☆☆ | ★★★★★ | ★★★★☆ | ★☆☆☆☆ | ★☆☆☆☆ |

Selected: automotive. See DECISIONS.md D-003.

---

## 3. The thesis

**Falsifiable statement:**

> For embedded automotive teams, producing MISRA compliance evidence
> costs more engineer-time and causes more anxiety than finding the
> violations themselves.

### Why the demand exists

ISO 26262 is not named in India's CMVR, but functions as mandatory: OEMs
and Tier-1s treat it as a contractual gate for program entry, and it is a
prerequisite for European and Asian export markets. Roughly half of
automotive teams comply because a customer mandated it.

Pressure is landing on India now. European OEM customers increasingly
require their Indian Tier-1 suppliers to demonstrate compliance. EV
battery management systems for two-wheelers and commercial vehicles
require functional safety assessment for cell balancing, thermal
management and charge control.

Scale: modern vehicles carry 200M+ lines of code; L5 autonomous
approaches 1B.

### Why the workflow is the gap

MISRA Compliance:2020 defines the evidence process:

- **GEP** — Guideline Enforcement Plan: how each guideline is enforced
- **GRP** — Guideline Re-categorization Plan: mandatory / required /
  advisory / disapplied, per project
- **Deviation records** — required rules permit deviation only with
  documented, justified, approved rationale
- **GCS** — Guideline Compliance Summary: declared compliance level per
  guideline

A deviation may cover a single violation or a group of similar violations
sharing a common use case, with common documentation plus a register of
locations.

The real work: triage tens of thousands of findings, cluster into
deviation use-cases, write rationales, get approval, produce an auditable
GCS. This is structurally identical to PPAP — assembling an auditable
evidence package for a customer.

---

## 4. Competitive landscape

### Direct incumbents (the actual competitor set)

| Vendor | Position | Weakness |
|---|---|---|
| Perforce Helix QAC | 100% MISRA C:2023/2025 and C++:2023, 96% AUTOSAR C++14. TÜV SÜD qualification kits for ASIL D, SIL 4, IEC 62304 Class C | Legacy UX, quote-based pricing, weak deviation workflow |
| LDRA | Full tool suite, certification pedigree | Expensive, dated, services-heavy |
| Parasoft C/C++test | TÜV SÜD certified | Broad but shallow on workflow |
| MathWorks Polyspace | Formal methods, strong | Locked to MATLAB ecosystem economics |

Market-reported pricing sits in the **$5,000–$15,000 per seat per year**
band. All quote-based and unpublished — **must be validated directly with
buyers during Phase 0.**

### Adjacent — validates the model, does not compete

ONEKEY and Finite State (see DECISIONS.md D-005) run this exact business
model in the CRA / IEC 62443 regime. The CRA exempts vehicles, so they do
not overlap today. ONEKEY covers UNECE R155 (cybersecurity), not ISO
26262 (functional safety) and not MISRA coding standards.

### Not competitors

GRC platforms (Vanta, Drata, Sprinto, Scrut, Secureframe) read
configuration via APIs, not code. Different layer entirely.

### Position

| Competitor | Their weakness | Our angle |
|---|---|---|
| Helix QAC / LDRA | Pricing, setup complexity, no AI, weak deviation workflow | Fair pricing, fast setup, AI-drafted rationales, audit-ready GCS |
| DIY (Cppcheck + clang-tidy) | No compliance artifacts at all | The evidence layer they cannot build |
