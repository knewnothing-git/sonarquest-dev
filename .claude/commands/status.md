---
description: Build progress and what is blocked
---

Read `graph/nodes.yaml` and `graph/state.json`. Print:

```
SonarQuest build — N/18 nodes done

PHASE 1 FOUNDATION      ###  3/3
PHASE 2 SCANNERS        #--  1/5   <- current: F-06
PHASE 3 CORE            ---  0/3
PHASE 4 SURFACES        ---  0/5
PHASE 5 SHIP            ---  0/2

Blocked:
  F-15  GitHub App key not configured

Next: F-06  SBOM generation (Syft)
      verify: pytest backend/tests/test_sbom.py -q
```

If nodes are blocked, list what would unblock each one in a single line.

$ARGUMENTS
