---
description: Show the next workable node and start it
---

Traverse the task graph and pick up the next unit of work.

## Steps

1. Read `graph/nodes.yaml` and `graph/state.json`.
2. A node is **workable** when all of:
   - every id in its `deps` has status `done`
   - its `gate` is `always`, or the gate expression evaluates true
     against `state.json`
   - its own status is `ready` or `active`
3. List every workable node, grouped by track, with its `verify`
   predicate shown.
4. If a workable node has `human_only: true`, **do not start it.** Print
   what the human needs to do and stop. Never simulate the outcome.
5. Otherwise, pick the highest-value workable node — prefer `spike` over
   `phase-0` prep, prefer nodes that unblock the most downstream work —
   and hand it to the agent named in its `agent` field.
6. Before writing any code: restate the node's `verify` predicate and say
   how you will evaluate it. If you cannot evaluate it, say so now rather
   than at the end.

## When nothing is workable

Say so plainly and show which gate is blocking. Do not invent adjacent
work to look productive, and do not propose starting a gated node "to
save time later." The gates are the design.

$ARGUMENTS
