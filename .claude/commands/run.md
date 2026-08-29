---
description: Run the build loop autonomously until blocked or complete
---

Build the product. Keep going without asking.

## Loop

```
while there is an unblocked node:
    node = next node whose deps are all done
    set state.run.current_node = node.id, status = active
    build it
    run node.verify
    if exit 0:
        ruff check + mypy on changed files
        commit "<node-id>: <what changed>"
        mark done, reset attempts
    else:
        attempts += 1
        if attempts <= 3: fix, retry
        else: mark blocked, record error, move to next node
    update graph/state.json
```

## Rules

- **Do not ask permission between nodes.** Decide using the policies in
  CLAUDE.md and continue.
- **Do not expand scope.** Anything extra goes to `BACKLOG.md`.
- **Log every autonomous choice** in `DECISIONS.md`: what you chose, what
  you rejected, one line of reasoning. This is how the decisions stay
  auditable without needing approval up front.
- **Never mark a node done on a failing verify.** The verify command is
  the only authority.
- If a node's verify turns out to be unrunnable or wrong, fix the verify
  in `graph/nodes.yaml`, log it, and continue.

## Hard stops — the only reasons to come back

1. A credential or API key is needed (GitHub App key, DB password)
2. An external service is unreachable after retries
3. Every remaining node is blocked

On a hard stop, print: what is done, what is blocked and why, and the
exact thing you need.

## Progress reporting

After each node, one line: `F-05 done — Semgrep runner, 14 tests green`.
No commentary between nodes. Full summary at the end of the run.

Scope: `$ARGUMENTS` (a node id, a phase, or empty for everything)
