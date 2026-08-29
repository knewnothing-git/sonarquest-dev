# Pricing, GTM, Roadmap, Risks

## 1. Pricing

Incumbents are quote-based and publish nothing. Market-reported ranges
put Helix QAC, LDRA, Parasoft and Polyspace at **$5,000–$15,000 per seat
per year**, usually with mandatory support and services attached.

**These numbers must be validated with real buyers in Phase 0.**

| Tier | Target | Price (to validate) |
|---|---|---|
| Community | OSS embedded, students, evaluation | Free, cloud, public repos |
| Team | EV startups, tier-2/3, ≤10 devs | ₹1.2–2L / dev / yr |
| Enterprise | Tier-1s, ESPs, on-prem | ₹3–5L / dev / yr + qualification kit |
| Qualification kit | Anyone doing ASIL work | ₹8–15L one-time |
| Services | Legacy MISRA remediation, audit prep | ₹25–75L per project |

**The pitch to a 25-engineer team:** roughly ₹30–50 lakh/year versus
₹2–3 crore with an incumbent. Not a discount — a budget-line
elimination. It gets the meeting every time.

**Services are not a distraction.** In this vertical they are 40–50% of
early revenue, they fund the product, and they teach you what to build.
The founder's quality-engineering background makes delivering them
credible. Take the revenue.

---

## 2. Go to market

### Wave 1 — Design partners (months 1–6)

**Indian EV startups.** Ather, Ola Electric, Ultraviolette, River, Simple
Energy, Matter, Vida.

New codebases, small teams, cost-sensitive, no incumbent contract to
displace. ASIL classification for BMS, motor controllers and braking ECUs
is non-negotiable for a credible safety case. They need this and cannot
afford the incumbents.

Offer: free for 12 months in exchange for design input and a logo.
Target 3–5.

### Wave 2 — Tier-2/3 suppliers (months 6–15)

Pune–Chakan–Aurangabad belt, plus Chennai and Coimbatore. Hundreds of
companies contractually required to show MISRA compliance to OEM
customers with zero budget for Helix QAC.

**This is the volume market and nobody is serving it.**

### Wave 3 — ESPs and Tier-1 India centres (months 15–30)

KPIT (Pune), Tata Elxsi, Tata Technologies, L&T Technology Services,
Cyient, Bosch Global Software Technologies, Continental, ZF, Marelli,
Harman.

9–18 month cycles, large contracts. Land in one project team, expand
across programs.

### Wave 4 — Adjacent safety-critical (month 24+)

Same engine, different rulepack, adjacent buyers:

- Medical devices — IEC 62304
- Industrial — IEC 61508
- Rail — EN 50128 / EN 50716
- Aerospace — DO-178C

Expansion, not distraction. Identical tech, adjacent standards.

### Channel

Indian distributors (e.g. GSAS) already resell Helix QAC with local
support and services. They know every buyer in the market. Once there are
3 reference customers, recruit one as a channel partner. **Highest
leverage GTM move available to a solo founder.**

### Where to find buyers

- **ARAI is in Pune.** Attend SIAT and their functional safety workshops.
- SAEINDIA Pune section — active, engineers actually attend
- ACMA events — wall-to-wall Tier-2/3 decision makers
- LinkedIn: `"functional safety" AND (MISRA OR "ISO 26262") AND India`
- Automotive Testing Expo India, embedded/ASPICE meetups

Roughly 60% of the target list is within a two-hour drive of Pune. An
in-person coffee converts at several times the rate of a video call in
this industry.

---

## 3. Build roadmap

```
PHASE 0 — Validation (weeks 1–4)   <-- DO NOT SKIP
1. 15 discovery calls               -> ≥10 confirm the pain thesis
2. IP lawyer on MISRA text          -> written opinion in hand
3. 3 design partner LOIs            -> signed, even if unpaid

PHASE 1 — Engine core (weeks 5–16)
4. Clang harness + build capture    -> parses a real AUTOSAR project
5. 40 decidable MISRA C:2012 rules  -> own test suite green
6. Fingerprinting + baseline        -> survives reformat + refactor
7. Benchmark vs Cppcheck addon      -> FP/FN rate measured, documented

PHASE 2 — Compliance workspace (weeks 17–30)   <-- THE PRODUCT
8.  GRP editor + multi-GRP          -> models a real project structure
9.  Deviation register + approvals  -> partner runs a real deviation
10. Violation clustering            -> 10k violations to <50 groups
11. GEP + GCS generator             -> an FS Manager calls it audit-ready

PHASE 3 — AI layer (weeks 31–42)
12. Rationale drafting              -> ≥70% accepted with minor edits
13. MISRA-safe fix generation       -> 0 new violations introduced
14. Legacy triage + audit rehearsal -> partner prefers it to manual

PHASE 4 — Platform hardening (weeks 43–54)
15. On-prem Docker Compose, air-gap -> installs with no internet
16. Jenkins / GitLab / Azure DevOps -> runs in partner's real pipeline
17. SSO, RBAC, audit log            -> passes partner IT security review

PHASE 5 — Commercial (weeks 55–72)
18. Qualification kit v1            -> FS consultant signs off
19. First 3 paid contracts          -> money in bank
20. Channel partner signed          -> first partner-sourced deal
```

**Realistic time to first revenue: 12–15 months.** This is not a 90-day
product.

---

## 4. Risks

| Risk | Severity | Mitigation |
|---|---|---|
| MISRA text copyright | Critical | Lawyer in Phase 0. Non-negotiable. |
| False positive rate | Critical | Automotive engineers are unforgiving. Ship fewer rules with higher precision. Publish the FP rate — nobody else does, and the transparency is itself a differentiator. |
| C/C++ analysis is genuinely hard | High | Clang does the heavy lifting. Scope to decidable rules first. |
| Long sales cycles | High | Services revenue bridges. EV startups close faster than Tier-1s. |
| Incumbent qualification lock-in | High | Attack new programs, not running ones. Nobody re-qualifies mid-program. |
| Solo founder capacity | High | This is a 2–3 year build. Be honest about that. |
| ONEKEY extends into functional safety | Medium | See DECISIONS.md D-007. Re-check quarterly. |
| Incumbent price-drops in response | Low | They won't — it would cannibalize their global book. Their pricing rigidity is our protection. |

---

## 5. Naming

**"SonarQuest" anchors to SonarQube, which is the wrong frame.** The
competitors are LDRA and Helix QAC, not Sonar. The association actively
costs credibility with a Functional Safety Manager.

Direction to consider: something in the register of safety and evidence —
*Aegis*, *Attestor*, *Vigil*, *Assure*.

Decision deferred until after Phase 0.
