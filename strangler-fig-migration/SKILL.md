---
name: strangler-fig-migration
description: Use when extracting a piece of functionality out of an existing monolith or legacy system while keeping it live and working throughout, rather than a big-bang rewrite. Trigger on requests like "how do I carve this out of the monolith", "plan a safe extraction", "migrate this incrementally", or "strangler fig this". Do NOT use for greenfield features with no existing code to migrate away from.
---

# Strangler Fig Migration Planning

Plan an incremental extraction of a piece of a monolith into a new
component, where the monolith stays fully live and functional throughout,
and callers are redirected gradually rather than all at once.

## When invoked

Use when the change is about carving a slice OUT of existing code, not
just adding a new feature. Pairs well with `volatility-mapping` for
choosing which slice to extract first.

## Steps

1. Identify the current callers of the code being extracted (who calls it,
   how, how often).
2. Design a facade/interface that existing callers can use UNCHANGED,
   sitting in front of both the old and new implementations.
3. Propose the smallest possible first slice to extract behind that
   facade — prefer the highest-volatility, lowest-blast-radius piece if a
   volatility mapping is available.
4. Describe the cutover plan: how traffic/calls move from old to new
   incrementally (e.g. percentage rollout, feature flag, shadow traffic),
   and how to roll back safely at each step if something breaks.
5. Confirm explicitly: does the old code path remain fully functional
   until the last caller is migrated? If not, redesign the facade before
   proceeding — this is the property that makes it a strangler-fig
   migration rather than a risky rewrite.

## Output format

```
### Extraction target: <name>
Current callers: ...
Facade design: ...
First slice to extract: ...
Cutover plan: ...
Rollback plan: ...
Old path stays live until fully migrated: yes/no — <justification>
```

## Guardrail

Never propose a plan where the old and new implementations must be cut
over atomically. If no incremental path exists, say so explicitly rather
than forcing one.
