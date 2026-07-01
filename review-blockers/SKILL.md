---
name: review-blockers
description: Use to catch review blockers (P1/P2 issues) either before or after code is written. Trigger before implementation on requests like "what will reviewers flag", "check for issues before coding", or "pre-mortem this design" — runs in PLAN mode. Trigger after code exists on requests like "double check this", "triple check this", "review your own work", or "check this before I open a PR" — runs in DIFF mode. If code already exists for the change being discussed, default to DIFF mode; otherwise use PLAN mode. Do NOT use for trivial one-line changes, and do not use as a substitute for actual human review.
---

# Review Blockers (Pre-Mortem + Self-Review)

One skill, two modes, same checklist categories — because the goal is
identical in both cases: surface what a strict senior reviewer would flag
as a P1/P2, either before the code exists (so it's built in correctly) or
after (so it's caught before a human reviewer catches it).

## Mode selection

- **PLAN mode**: no implementation exists yet for this change. Produce a
  checklist the implementation must satisfy.
- **DIFF mode**: implementation already exists (a diff, a just-written
  file, existing code). Review it against the checklist directly.

If it's ambiguous which mode applies, ask once, or infer from context
(e.g. if the user just asked you to write code and now says "double check
it," that's DIFF mode).

## Checklist categories

Only include categories actually relevant to the change; state briefly why
a category doesn't apply rather than omitting it silently.

- Null / empty / missing / malformed inputs
- Concurrency and race conditions
- Auth / authz gaps
- Error handling and failure propagation (what happens when a dependency
  fails?)
- Backward compatibility with existing callers
- Data consistency across any boundary being introduced or touched
- Performance / scaling implications
- Security (injection, unsanitized input, secrets handling)

## PLAN mode steps

1. For each relevant category, state what could go wrong and how the
   implementation should handle it.
2. Output as a checklist the implementation must satisfy.

```
- [ ] <Risk>: <how it should be handled>
```

## DIFF mode steps

1. Adopt the persona of a strict, skeptical reviewer who did not write
   this code and has no investment in it looking good.
2. If a checklist already exists for this change, review against it
   directly. If not, derive one from the categories above first.
3. Go through the diff against each item: satisfied / not satisfied /
   partially satisfied, with a one-line reason.
4. Identify any NEW risk introduced by the implementation that wasn't
   anticipated (e.g. a new dependency, a new failure path).
5. Assume this will be rejected unless you can prove otherwise — do not
   default to "looks good."

```
- [x] <item> — satisfied: <reason>
- [ ] <item> — NOT satisfied: <reason, what's missing>

New risks introduced not on original checklist:
- <risk> — <mitigation or note that it's unmitigated>

Verdict: ready for review / needs fixes before review
```

## Guardrail

In DIFF mode, if the verdict is "needs fixes before review," list the
required fixes explicitly rather than softening it or proceeding as if the
change were done. In PLAN mode, incompleteness in the checklist defeats
the purpose of running this before implementation — don't skip categories
to save space.
