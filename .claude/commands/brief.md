---
description: Plain-language project status and the decisions waiting on you. Start every session here.
---

Give Yogesh a short briefing. He is the decision maker, not an observer —
this command exists so he can steer without reading code.

## Output — keep it under 30 lines total

**1. Where things stand** (3–4 sentences, plain language)

No node ids in this section. No jargon. "You've logged 4 of 15 calls.
The prototype can now read a codebase and list violations, but nothing
produces an audit document yet."

**2. Decisions waiting on you**

For each one:
- The question, in one sentence, in his language
- What each choice costs or buys
- Your recommendation and why — but make clear it is his call

If there are none, say so.

**3. What I can do next without asking**

Only work he could spot-check himself. List it, don't start it.

**4. What is blocked and why**

One line each, in plain language. "Nothing can be built for real
customers until the 15 calls are done — that's the gate you set."

**5. Anything I'm uncertain about**

Be specific. "I picked FreeRTOS as the test codebase but I'm not sure
it's representative of what a Tier-2 supplier's ECU code looks like —
you'd know better than me."

## Rules

- Translate everything. If you write a term he'd have to look up,
  rewrite the line.
- Never present a technical metric as a decision. Turn it into a
  question he can answer from his own experience.
- Never say a node is done if its verification was your own judgment on
  something he asked you to build. Say "I think this is done — here's the
  evidence, you decide."
- If the honest brief is "nothing has progressed since last time," say
  that. Do not manufacture momentum.

$ARGUMENTS
