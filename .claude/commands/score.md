---
description: Tally Phase 0 results against the locked threshold
---

Produce the go/no-go tally. This is node P0-06.

## Steps

1. Read every file in `validation/calls/`. Count them.
2. If fewer than 15, say so and **stop**. A tally on partial data invites
   moving the threshold. Report progress instead.
3. Tally the 6-point score per call. Print the full distribution, not
   just the headline.
4. Print every Q12 answer verbatim, grouped: in the tool / Excel / Word /
   SharePoint / doesn't exist.
5. Print the hypothesis breakdown H1–H6: how many calls confirmed each.
6. State the count scoring 4+ against `phase0.threshold` (8).
7. State the outcome in one sentence, using the pre-registered bands:
   - `>= 8` → thesis holds
   - `5-7` → partial; pain real but narrower; re-segment before building
   - `<= 4` → thesis failed

## Rules

- The threshold is locked (`threshold_locked: true`). If asked to lower
  it now, refuse and quote this line back.
- Do not recommend a decision. Present the number.
- If the outcome is negative, say so in the first sentence. Do not open
  with encouraging context.
- The human writes `phase0.decision`. You never do.

## After the human decides

Hand to the `scribe` agent to append a `D-NNN` entry recording the
decision, the tally that drove it, and its reversal condition.

$ARGUMENTS
