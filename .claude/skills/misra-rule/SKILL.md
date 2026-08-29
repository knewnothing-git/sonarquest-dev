---
name: misra-rule
description: Procedure for implementing and testing a single MISRA rule on the Clang AST. Use whenever adding a rule to the spike analyzer.
---

# Implementing a MISRA rule

One rule, one procedure. Follow it in order.

## 0 — Copyright check, before anything else

You may write: the rule identifier (`Rule 8.4`), your own description of
what the rule is trying to prevent, and code examples you wrote yourself.

You may not write: MISRA guideline text, the official rationale text, the
official amplification text, or a close paraphrase of any of them — in
source, comments, tests, docs, commit messages, or tool output.

If you are unsure whether a sentence is a paraphrase, do not write it.

## 1 — Classify the rule

State these before writing code:

| Property | Values |
|---|---|
| Category | Mandatory / Required / Advisory |
| Decidability | Decidable / Undecidable |
| Scope | Single translation unit / System |

**Only implement decidable + single-TU rules in the spike.** Anything
else needs the data flow layer that does not exist yet. If the rule you
picked turns out to be undecidable, stop and pick another — do not ship
an approximation.

## 2 — Write negative tests first

Create `spike/tests/rule_NN_negative.c` with code that is compliant and
must produce **zero** findings. Include the cases most likely to trip a
naive matcher:

- macro-expanded code
- code inside `#if 0` and inactive preprocessor branches
- system headers and third-party headers
- generated code
- unusual but legal constructs (K&R declarations, bit-fields, anonymous
  unions, `_Generic`)

Over-firing is the dominant failure mode. Write these first so the
matcher is built against them rather than patched afterwards.

## 3 — Write positive tests

`spike/tests/rule_NN_positive.c` — genuine violations, one per case, each
annotated with the expected finding location.

## 4 — Implement the matcher

- Use `ASTMatchers` where the pattern is structural.
- Use a `RecursiveASTVisitor` only where matchers cannot express it.
- Skip nodes from system headers explicitly. This is the single most
  common source of noise.
- Guard against macro-expansion locations unless the rule is about
  macros.

## 5 — Emit the finding

Every finding carries:

```
rule_id            e.g. "MISRA-C-2012-8.4"
fingerprint        (rule_id, normalized_AST_context_hash, enclosing_symbol)
file, line, col    for display only, never for identity
enclosing_symbol   function or file scope
severity           from category
```

The fingerprint must not change when the file is reformatted. Test this.

## 6 — Run the whole suite

Not just the new tests. A new matcher that breaks an old rule's negative
cases is a regression, and it will not be caught later.

## 7 — Report honestly

State: rule id, category, decidability, test counts, and any construct
you know it handles badly. A documented limitation is worth more than a
silent one — the market this product serves audits its tools.

## Checklist

- [ ] No MISRA guideline text anywhere in the change
- [ ] Rule is decidable and single-TU
- [ ] Negative tests written before the matcher
- [ ] System headers excluded
- [ ] Macro expansion handled
- [ ] Fingerprint stable under clang-format
- [ ] Full suite green, not just new tests
- [ ] Known limitations documented
