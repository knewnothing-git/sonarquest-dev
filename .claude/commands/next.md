---
description: Build exactly one node, then stop
---

Same as `/run` but stops after a single node. Use when you want to
inspect the result before continuing.

1. Read `graph/nodes.yaml` and `graph/state.json`
2. Pick the node named in `$ARGUMENTS`, or the next unblocked one
3. Build it
4. Run its `verify`
5. On green: lint, commit, mark done
6. On red after 3 attempts: mark blocked with the error
7. Print the result and the next node that would run

Then stop.

$ARGUMENTS
