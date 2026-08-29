---
description: Show gate status and what each gate is blocking
---

Report the state of every gate and its downstream impact.

## Steps

1. Read `graph/nodes.yaml` gates and `graph/state.json`.
2. For each gate, print:
   - the expression
   - the current values it depends on
   - PASS / FAIL / UNMEASURED
   - the list of node ids it currently blocks
3. Flag anything `unset` or `null` that a gate depends on. Unmeasured is
   not the same as failing, and neither is the same as passing — do not
   collapse them.

## Output shape

```
phase0_go        FAIL (decision=unset)
  blocks: PR-01 PR-02 PR-03 PR-04

legal_clear      UNMEASURED (misra_opinion=unset)
  blocks: PR-04

spike_green      UNMEASURED (fp_rate_measured=false)
  blocks: SP-06
```

Then one line per blocked track saying what would unblock it soonest.

Do not editorialize about whether a gate should be relaxed.

$ARGUMENTS
