# Phase 0 — Validation Kit

Four weeks. 15 calls. One go/no-go decision. No code until this passes.

**Caveat:** company names below come from general knowledge of the Indian
automotive landscape. Verify current status, site presence and who holds
these roles — org charts change.

---

## 1. Hypotheses

| # | Hypothesis | Confirmed if you hear |
|---|---|---|
| H1 | Documentation effort > detection effort | Person-day estimates for deviation write-ups exceed fixing time |
| H2 | **Tool compliance reporting is inadequate** | **They maintain the GCS in Excel or Word alongside a paid tool** |
| H3 | Tier-2/3 and EV startups are priced out | "We looked at LDRA/QAC and walked away" |
| H4 | A buying trigger exists | OEM mandate, new program, export push, failed audit |
| H5 | Budget and identifiable buyer | They can name who signs |
| H6 | Tool qualification is a gate | "We couldn't use X because it wasn't qualified" |

**H2 is the kill shot.** If a company paying ₹50 lakh/year for Helix QAC
still assembles its GCS by hand in Excel, the thesis is confirmed in one
answer. Ask it in every call.

---

## 2. Call script (30 min)

**The one rule: never pitch.** You are a researcher, not a founder. The
moment you describe a product they switch to encouragement mode and the
data becomes worthless.

### Opening (2 min)

> Thanks for the time. Quick context — I spent about a decade in
> automotive quality and precision metrology, and I'm researching how
> embedded teams in India actually handle coding-standard compliance.
> I'm not selling anything and I don't have a product. Mind if I record
> for my notes?

### Block A — Context (5 min)

1. Tell me about the last ECU or controller project you shipped. What was
   the software scope?
2. Which coding standard applied — MISRA C, C++, AUTOSAR? Who required
   it: an OEM, internal policy, an export market?
3. What ASIL level were you working to?

*Listening for:* customer-mandated (good) vs self-imposed (weaker).

### Block B — Current process (7 min)

4. Walk me through everything from "code is written" to "we can claim
   MISRA compliance." Every step, including the boring ones.
5. What tools are in that chain?
6. **Where does the tool stop and manual work begin?**
7. Who owns the compliance artefacts — developers, quality, or functional
   safety?

*Listening for:* the handoff point. That gap is the product.

### Block C — Pain excavation (10 min) — the meat

8. On the last program, roughly how many violations did the tool report
   on the first full run?
9. Of those, how many did you fix versus deviate?
10. **Walk me through raising one single deviation. Who writes the
    rationale, who reviews, who approves, where does the record live?**
11. How long did the deviation write-ups take across the program, in
    person-days?
12. **Where does your Guideline Compliance Summary live right now? Can
    you describe the file?**  <-- the H2 question
13. Last assessment or customer audit — what did the assessor push back
    on?
14. What had to be redone because of that pushback?
15. If your current tool vendor could fix exactly one thing, what would
    it be?

*Listening for:* "Excel", "Word", "SharePoint", "manual", "copy-paste",
"template", "the night before".

### Block D — Economics (5 min)

16. Order of magnitude, what does the current toolchain cost per year —
    lakhs or crores?
17. Who signs off on that spend?
18. How many engineers touch the compliance process, even part-time?
19. Roughly how many person-days went into audit prep last program?

*If they deflect on cost, don't push.* Ask instead: "Did cost ever stop
you adding a tool you wanted?"

### Block E — Triggers & qualification (4 min)

20. What made you adopt MISRA originally — a specific event or a customer
    demand?
21. When would you next realistically evaluate a toolchain change?
22. Anything upcoming — new program, new OEM customer, export push — that
    changes the requirement?
23. Do you need the analysis tool itself qualified? Has qualification ever
    blocked you from using something you wanted?

### Close (2 min)

24. **Who else should I be talking to about this?**
25. Can I come back in a few months when I have something to show?

**Q24 is how 15 calls becomes 30. Ask it every single time.**

---

## 3. Scoring sheet

Fill in within 30 minutes of each call, while it's fresh.

| Field | Capture |
|---|---|
| Company / segment | EV startup · Tier-2/3 · Tier-1/ESP · Consultant |
| Role | |
| Current tool | QAC · LDRA · Parasoft · Polyspace · PC-lint · PVS · C-STAT · Cppcheck · none |
| Violations on first run | |
| Fixed vs deviated | |
| **GCS lives in** | **Tool · Excel · Word · SharePoint · Doesn't exist** |
| Deviation effort (person-days) | |
| Audit prep effort (person-days) | |
| Annual toolchain spend | |
| Who signs | |
| Qualification required? | Y / N / ASIL level |
| Trigger event present? | Y / N — describe |

### Score 0–6, one point each

- [ ] Deviation/documentation effort exceeds fixing effort
- [ ] GCS maintained outside their analysis tool
- [ ] Named a specific audit pushback that caused rework
- [ ] Ruled out a commercial tool on price
- [ ] Named the budget holder without hesitation
- [ ] Has a concrete trigger event in the next 12 months

**5–6 = strong · 3–4 = warm · 0–2 = thesis miss**

### Go / No-Go

- **≥8 calls scoring 4+** → thesis holds. Proceed to Phase 1.
- **5–7 calls scoring 4+** → partial. Pain is real but narrower.
  Re-segment before building.
- **≤4 calls scoring 4+** → **stop.**

**Write this threshold down before the calls begin.** The temptation to
move the goalposts after four weeks of investment is enormous.

---

## 4. Target list

### Tranche 1 — Functional safety consultants (call FIRST, aim 2–3)

They see 20+ companies a year and give you the landscape in two calls.
Several are potential channel partners later.

Embitel (Bangalore) · GSAS India (Helix QAC distributor — learn the
incumbent's motion from inside) · ElectRay · TÜV SÜD India · TÜV
Rheinland India · TÜV NORD India · Exida India · Ricardo India · Embien
Technologies (Chennai) · Tessolve

*Roles:* Functional Safety Consultant, Lead Assessor, Practice Head —
Automotive Software

### Tranche 2 — EV companies (aim 5)

**Pune (go in person):** Sedemac Mechatronics (ECUs and controllers,
ideal profile) · Tork Motors · Kinetic Green · Vayve Mobility

**Elsewhere:** Ather Energy · Ultraviolette · River · Simple Energy · Ola
Electric (Bangalore) · ION Energy (Mumbai — pure BMS software, very high
fit) · Matter (Ahmedabad) · Exponent Energy · Log9 · Vida/Hero

*Roles:* Head of Embedded Software · Functional Safety Manager · VP
Engineering · CTO · BMS Software Lead

### Tranche 3 — Tier-2/3 suppliers (aim 5) — the volume market

Varroc Engineering (Aurangabad) · Endurance Technologies (Aurangabad) ·
Uno Minda · Minda Corporation · Sona Comstar · Napino Auto & Electronics
· Lumax · Sansera · Pricol (Coimbatore) · Rane Group (Chennai)

*Roles:* Software Quality Manager · Embedded Software Manager ·
ASPICE/Process Lead · Head of Electronics R&D

### Tranche 4 — Tier-1s and ESPs (aim 2–3)

Landscape, not early revenue.

KPIT Technologies (Pune HQ) · Tata Technologies (Pune HQ) · Tata Elxsi ·
L&T Technology Services · Cyient · Bosch Global Software Technologies ·
Continental · ZF · Marelli · Valeo · Harman

*Roles:* Delivery Head — Automotive Software · Functional Safety Manager
· Practice Head — Embedded

### Extra call worth adding

Someone who has used ONEKEY or Finite State. Ask what the platform does
well and where it stops. Free product design.

---

## 5. Outreach templates

### A — Cold LinkedIn

> Hi [Name] — I spent ~10 years in automotive quality and precision
> metrology (Renishaw), and I'm researching how Indian embedded teams
> handle MISRA and ISO 26262 compliance evidence in practice, not in
> theory.
>
> Not selling anything — no product, no pitch. I'm trying to understand
> where the real effort goes between running the analysis and surviving
> the audit.
>
> Would you be open to 30 minutes? Happy to share what I learn across all
> the conversations.

### B — Warm intro request

> [Name], I'm researching MISRA compliance workflows in Indian automotive
> — specifically how much manual effort goes into deviation documentation
> and audit prep.
>
> You're connected to [Person] at [Company]. Would you be comfortable
> introducing us? Purely research, 30 minutes, nothing to sell.

### C — Consultant / channel angle

> Hi [Name] — you assess a lot of teams against ISO 26262. I'm
> researching one question: across your engagements, is the harder
> problem finding violations, or assembling defensible compliance
> evidence?
>
> Ex-automotive quality background, independent research. 30 minutes, and
> I'll share the aggregate findings with you.

### Volume math

For 15 calls, plan on ~80–100 LinkedIn requests with notes, 15–20 warm
intro asks, 2 in-person events. Expect ~20% response, ~60% of those
converting.

---

## 6. Legal track (parallel, week 1)

### Step 1 — Email MISRA directly, day one

Via misra.org.uk. Free, ten minutes, may resolve the biggest structural
risk before a rupee is spent on legal fees. Ask:

1. Is a licence required to reference MISRA rule identifiers in a
   commercial tool?
2. Is a licence required to include or paraphrase guideline text?
3. Do you operate a licensing programme for tool vendors, on what terms?
4. Trademark usage requirements when describing MISRA compliance
   checking?

### Step 2 — IP counsel brief

1. Can we reference rule identifiers commercially without licence?
2. Can we publish **original** descriptions of rule intent?
3. Does a customer-supplied-rule-text model shift or create liability?
4. Trademark: is "MISRA C:2012 compliance checking" permissible, or must
   it be "checks against MISRA C:2012 guidelines"?
5. Same analysis for AUTOSAR C++14 and CERT C/C++
6. Jurisdiction exposure: India, UK (MISRA), EU/US customers

Firms sized for a solo founder: Spice Route Legal, Ira Law, Saikrishna &
Associates. Budget ₹50k–1.5L for a written opinion. **Get it in writing**
— enterprise procurement will ask for it later anyway.

---

## 7. Design partner LOI

One page, non-binding. Testing commitment, not extracting a contract.

**They give:** one named contact, 2 hrs/month for 12 months; access to a
representative codebase (NDA'd or sanitised); feedback on compliance
artefact drafts; logo permission once live.

**You give:** free licence 24 months; roadmap input; founder-level
support.

**Deliberately omits:** any purchase obligation. If they won't sign even
this, the pain isn't acute enough — which is exactly the signal you're
paying for.

---

## 8. Four-week plan

```
WEEK 1 — Setup
· Email MISRA                        -> sent day 1
· Book IP counsel consult            -> on calendar
· Build 60-name target list          -> names, roles, companies
· LinkedIn headline leads with
  automotive quality credential      -> live
· Outreach wave 1 (40 msgs,
  consultants + EV first)            -> sent

WEEK 2 — First calls
· Calls 1–5                          -> scored within 30 min each
· Outreach wave 2 (40 msgs)          -> sent
· Attend one ARAI/SAEINDIA event     -> 3 business cards
· First read on the "Excel signal"   -> counted

WEEK 3 — Volume
· Calls 6–12                         -> scored
· Chase referrals from Q24           -> ≥5 new names
· IP counsel written opinion         -> in hand
· Mid-point score check              -> on track, or pivot early

WEEK 4 — Decide
· Calls 13–15                        -> scored
· Tally against threshold            -> number written down
· Request 3 design partner LOIs      -> asked, not hinted
· GO / NO-GO                         -> decided, not deferred
```

---

## 9. Three traps

1. **Pitching.** The instant you say "I'm building a tool that…", they
   switch to encouragement. If you feel the urge, ask Q10 again instead.
2. **Talking only to developers.** Developers feel violation pain;
   managers feel audit pain and hold budget. At least 10 of 15 should be
   manager-level or above.
3. **Moving the threshold.** Five strong signals instead of eight means
   "narrower than I thought — re-segment," not "close enough."
