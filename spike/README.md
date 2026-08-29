> **DEFERRED — not staffed, not scheduled.**
>
> This track was replaced by the thin Python prototype. See
> `DECISIONS.md` D-009 and the `prototype` track in `graph/nodes.yaml`.
>
> Reason: the decision maker on this project is a quality engineer, not a
> compiler engineer. A track whose outputs are AST matchers and precision
> intervals cannot be independently verified by him. The C++ work returns
> post-revenue as a staffed project, at node PR-02.
>
> Everything below is preserved for that point. None of it has been built.
# Engine Spike

**Status: scaffolding only. Never compiled. Node SP-01 is to make it build.**

## Purpose

De-risk the single hardest technical assumption in the project:

> Can a Clang-based analyzer implement decidable MISRA rules at a
> false-positive rate low enough that automotive engineers will tolerate
> it, with fingerprints stable enough that approved deviations survive
> refactoring?

If the answer is no, the product does not exist regardless of what Phase 0
says about market demand. This track therefore runs in parallel with
Phase 0 rather than after it.

## What "done" means

`SP-06` ships a container that takes a repository path and emits a JSON
violation report with stable fingerprints, alongside a documented,
measured precision figure.

That is the deployable artifact. It is not the product — it is proof the
product is buildable.

## Exit criteria

| Node | Gate |
|---|---|
| SP-01 | Docker build succeeds, binary runs |
| SP-02 | Parses >= 95% of translation units in a real ECU codebase |
| SP-03 | 10 decidable single-TU rules, own suite green |
| SP-04 | 0 fingerprint changes under clang-format |
| SP-05 | Precision measured on n>=100 random adjudicated findings |
| SP-06 | `spike_green` gate: measured FP rate <= 15% |

**If SP-05 returns a precision that fails the gate, that is a real
result.** Report it, record it in `DECISIONS.md`, and reassess whether
Clang alone is sufficient or whether the data flow layer has to come
earlier than planned. Do not adjust the threshold to pass.

## Corpus

SP-02 requires an automotive-representative C codebase with a buildable
`compile_commands.json`. Candidates to evaluate:

- FreeRTOS + a vendor HAL
- An open-source BMS or motor controller firmware project
- An AUTOSAR-style OSS stack

Requirements: real embedded C, non-trivial preprocessor use, vendor
headers, and enough volume that a 100-finding random sample is meaningful.
Record the choice in `graph/state.json` under `spike.corpus`.

## Layout

```
spike/
  docker/Dockerfile     LLVM/Clang LibTooling + CMake build env
  src/                  analyzer source
  tests/                per-rule positive and negative cases
  reports/              fp-rate.md and other measurements
```

## Rules on rules

Read `.claude/skills/misra-rule/SKILL.md` before adding any rule. The
copyright constraint in step 0 is not optional and not a style guideline.
