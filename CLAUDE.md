# SonarQuest — Agent Operating Context

You are working on SonarQuest: a compliance evidence platform for
safety-critical embedded software (MISRA C/C++, ISO 26262).

**Read this file fully before acting. Then read `graph/state.json`.**

---

## Prime directive

This project is **pre-validation**. The commercial thesis has not been
confirmed. Work is governed by a task graph with hard gates, not by a
roadmap you can improvise against.

**Never start work on a node whose gate is unmet.** If asked to, refuse
and say which gate blocks it. The gates exist because building the wrong
thing for twelve months is the failure mode this project is designed to
avoid.

---

## What this product is

Static analysis tools find MISRA violations. They are weak at what
happens next: triaging thousands of findings, clustering them into
deviation use-cases, writing justification rationales, routing approvals,
and producing an auditable Guideline Compliance Summary that survives an
OEM audit.

That downstream workflow is the product. The scanner is table stakes.

**Competitors:** LDRA, Perforce Helix QAC, Parasoft, MathWorks Polyspace.
**Not** SonarQube, DeepSource, or Codacy. If you find yourself reasoning
about developer-experience code quality, you have drifted.

Full reasoning in `DECISIONS.md` and `docs/`. Read `DECISIONS.md` before
proposing any strategic change — it records what was already rejected and
why, including the analytical error that produced one bad recommendation.

---

## How work is selected

```
graph/nodes.yaml    the DAG — definition of every work unit
graph/state.json    current status per node — the source of truth
```

Each node carries:

| Field | Meaning |
|---|---|
| `gate` | Condition that must hold before work may start |
| `do` | The work |
| `verify` | Objective predicate that must pass to mark done |
| `agent` | Which subagent owns it |
| `human_only` | Claude cannot execute this — surface it, don't simulate it |

Run `/next` to get the next unblocked node. Run `/gate` to see what is
blocking what.

**A node is done only when `verify` passes.** Not when the code looks
right, not when tests you wrote yourself pass, not when you believe it
works. If `verify` cannot be evaluated, the node is not done — say so.

---

## Tracks

| Track | Status | Notes |
|---|---|---|
| `phase-0` | **Active** | Market validation. Mostly `human_only`. You prepare, synthesize, and score — you do not make calls or invent responses. |
| `spike` | **Active** | Engine de-risking. Runs in parallel, does not depend on Phase 0. This is where most of your work is. |
| `product` | **Blocked** | Every node gated on `phase0.decision == "GO"`. Do not touch. |

---

## Hard rules

1. **Never fabricate Phase 0 data.** Call notes come from the human. If
   `validation/calls/` is empty, the answer to "what did we learn" is
   "nothing yet."

2. **MISRA guideline text is copyrighted** (HORIBA MIRA). Never reproduce
   rule text in code, comments, tests, docs, or output. Reference rule
   identifiers only (`Rule 8.4`), and write original descriptions of
   intent. This is a company-ending mistake, not a style preference.

3. **False positives are the product risk.** A rule that fires wrongly is
   worse than a rule that does not exist. Every rule ships with negative
   test cases. Precision beats coverage in this market, always.

4. **Fingerprint violations on `(rule_id, normalized_AST_context_hash,
   enclosing_symbol)`. Never on line number.** If a fingerprint changes
   on reformat, every approved deviation breaks and the customer never
   trusts the tool again.

5. **Surgical changes only.** Touch what the node requires. Do not
   refactor adjacent code, do not improve comments you did not write, do
   not delete pre-existing dead code — mention it instead.

6. **State assumptions before implementing.** If a node is ambiguous or
   two readings exist, stop and say so. Do not pick one silently.

7. **Simplicity first.** No abstraction for single-use code. No
   configurability nobody asked for. No error handling for impossible
   states. If it could be half the size, rewrite it.

---

## Repository map

```
CLAUDE.md              this file
DECISIONS.md           append-only decision log
graph/nodes.yaml       task DAG
graph/state.json       node status — source of truth
docs/                  strategy, product, GTM, Phase 0 kit
validation/            Phase 0 artifacts (human-sourced)
spike/                 engine spike — Clang/LLVM MISRA analyzer
.claude/agents/        subagent definitions
.claude/commands/      slash commands
.claude/skills/        repeatable procedures
```

---

## Recording decisions

Any strategic choice, rejection, or reversal gets an entry in
`DECISIONS.md` via the `scribe` agent. Format: `D-NNN`, date, status,
reasoning, and what would reverse it. Append only — never edit or delete
a prior entry, even a wrong one. The wrong ones are the useful ones.

---

## Tone

Report what is true, including when it is unwelcome. If the spike shows
the false-positive rate is unacceptable, say so plainly. If a Phase 0
tally misses the threshold, say the thesis failed. Do not soften a
negative result into a "partial success" — the whole point of the gates
is that they can fail.
