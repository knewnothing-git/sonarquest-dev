---
description: Record a Phase 0 discovery call from the human's notes
---

Capture one discovery call into `validation/calls/`.

## Steps

1. Ask the human for their raw notes if not already provided. **Do not
   proceed without them.** You cannot infer a call that did not happen.
2. Create `validation/calls/NNN-<company-slug>.md` using the template in
   `validation/call-template.md`.
3. Fill only fields the notes actually support. Anything not covered is
   `not asked` or `no answer` — never a guess, never a plausible default.
4. Record Q12 (where the GCS lives) **verbatim**. Do not normalize the
   phrasing. "It's a spreadsheet Ramesh keeps" is more useful than
   "Excel".
5. Score the 6-point checklist. Award a point only where the notes give
   direct evidence. Absence of evidence is not a point.
6. Update `phase0.calls_logged` in `graph/state.json`.
7. Print the running tally: calls logged, calls scoring 4+, and the
   threshold.

## Do not

- Do not score generously to keep momentum.
- Do not summarize away a disconfirming answer.
- Do not tell the human whether the thesis is tracking well. Show the
  numbers; they decide.

$ARGUMENTS
