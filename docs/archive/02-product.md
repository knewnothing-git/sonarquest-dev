# Product Definition & Architecture

Scope below is v1.0, not MVP.

---

## Module 1 — Analysis engine

- MISRA C:2012 / C:2023 rule checking (decidable, single-TU rules first)
- MISRA C++:2023 (phase 2)
- Cross-translation-unit data flow for system-level rules
- Compiler-aware parsing: GCC, Clang, IAR, Tasking, Green Hills, ARM
- Baseline mode — suppress pre-existing, enforce on new code only
- Incremental analysis on diff

## Module 2 — Compliance workspace (the moat)

- **GRP editor** — per-project, per-component, versioned
- **Multiple GRP support** — native vs. adopted/third-party code, which
  MISRA:2020 explicitly permits
- **Deviation register** — raise → rationale → risk assessment →
  reviewer → approver → expiry
- **Deviation permits** — reusable org-level pre-approved use cases
  (e.g. integer-to-pointer cast for hardware register access)
- **Violation → deviation clustering** — thousands of violations into a
  handful of use-cases
- **GEP generator** — auto-derived from tool configuration
- **GCS generator** — one click, audit-ready, matching the MISRA:2020
  compliance-level matrix (Compliant / Deviations / Violations /
  Disapplied)

## Module 3 — AI layer (the differentiator)

- **Deviation rationale drafting** — AI writes first draft, engineer
  edits and approves. Saves weeks.
- **MISRA-safe fix generation** — every suggested fix is re-analyzed
  before being offered. Generic AI fixes routinely introduce new
  violations; this is the differentiator.
- **Legacy triage** — 500k LOC with 40,000 violations: cluster, surface
  the ~200 carrying real safety risk, group the rest into deviation
  classes.
- **Contextual rule explanation** — why this terse legalistic rule
  matters in *this* code.
- **Audit rehearsal** — AI plays the OEM assessor and attacks the
  weakest deviation rationales before the real audit does.

## Module 4 — Safety case integration

- ISO 26262-6 Table 1 method traceability
- Complexity metrics with ASIL-appropriate thresholds
- Requirements traceability hooks: Polarion, Jama, DOORS, Codebeamer
- Evidence export bundle for functional safety assessment

## Module 5 — Platform

- CI/CD: Jenkins (dominant in automotive), GitLab CI, Azure DevOps,
  GitHub Actions
- **On-premise / air-gapped deployment — mandatory, non-negotiable**
- SSO / LDAP, audit log, role separation
  (developer / reviewer / approver / assessor)
- IDE plugins: VS Code, Eclipse-based (many automotive IDEs are Eclipse
  forks)

## Module 6 — Qualification support

- Tool qualification kit per ISO 26262-8 Clause 11
- Validation test suite and results
- Safety manual / tool operational requirements
- TCL determination guidance

---

## Architecture

```
Ingestion: git + build capture
(compile_commands.json, bear, or compiler wrapper)
              |
              v
Parse layer — Clang/LLVM front end
· AST + CFG + preprocessor trace
· Compiler dialect emulation (IAR / Tasking / GHS)
              |
              v
Rule engine
· Tier 1: AST matcher rules (decidable, single TU)
· Tier 2: Data-flow rules (Joern CPG / IFDS)
· Tier 3: System rules (link-time whole program)
· Tier 4: AI-assisted undecidable rules
              |
              v
Violation store — Postgres
· Stable fingerprinting (survives refactors)
· Baseline / diff engine
              |
              v
Compliance engine   <-- THE PRODUCT
· GRP resolver · Deviation register
· Approval workflow · GEP/GCS generator
              |
              v
AI service (Claude API)
· rationale drafting · clustering
· MISRA-safe fixes · audit rehearsal
· on-prem: BYO-key or local model
```

### Stack

| Layer | Choice | Note |
|---|---|---|
| Analysis core | C++ on Clang/LLVM | Apache-2.0 with LLVM exception — commercially safe |
| Data flow | Joern (Apache-2.0) or custom IFDS on Clang CFG | |
| Orchestration | Python (FastAPI) + Celery | |
| DB | Postgres + pgvector | pgvector for violation similarity clustering |
| Frontend | React + Tailwind | Will look premium next to LDRA's dated UI |
| Deploy | Docker Compose (on-prem) + K8s (cloud) | |

### Critical constraint: stable violation fingerprinting

If a violation ID changes when a file is reformatted, every approved
deviation breaks and customer trust is permanently lost.

Fingerprint on `(rule_id, normalized_AST_context_hash, enclosing_symbol)`.
**Never on line number.**

---

## Rule engine sequencing

Do not attempt 100% coverage. Helix QAC has it; matching them in year one
is not possible. Be honest about coverage.

| Wave | Scope | Effort |
|---|---|---|
| 1 | ~40 Mandatory + high-frequency Required, decidable, single-TU | 6–8 wks |
| 2 | ~60 more Required rules, decidable | 8 wks |
| 3 | Data-flow / undecidable rules | 12 wks |
| 4 | System-level rules | 8 wks |
| 5 | MISRA C++:2023 | 16 wks |

Build the validation suite from day one — positive cases, negative cases,
known-tricky edges. **This test suite later becomes the tool
qualification evidence.** Write it as if an auditor will read it.

---

## Legal constraints — do not skip

**MISRA guideline text is copyrighted** by MISRA (HORIBA MIRA). Rule text
cannot be reproduced without a licence. This is why Cppcheck's MISRA
addon requires the user to supply their own MISRA PDF.

Options:
1. Reference rule numbers only; write original descriptions of intent
2. License the text from MISRA
3. Require customer-supplied rule text at configuration time

Pragmatic path: 1 + 3 combined. **Get IP counsel review before shipping
any code.** This is the class of mistake that kills a company at the
moment it starts working.

**Tool qualification (ISO 26262-8 Clause 11):** full TÜV certification is
realistically EUR 30k–80k and 6–12 months. Do not start there.

1. Ship a **qualification kit** — validation suite, safety manual, tool
   classification guidance — so the *customer* qualifies SonarQuest in
   their own context. Standard practice, dramatically cheaper.
2. Sell into **QM and ASIL-A/B** work first (infotainment, telematics,
   body electronics, diagnostics) where the bar is lower.
3. Fund TÜV certification from revenue at 15–20 paying customers.
