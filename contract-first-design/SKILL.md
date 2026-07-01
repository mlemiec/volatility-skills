---
name: contract-first-design
description: Use after volatility axes/boundaries have been identified for a feature or component, to define stable interfaces before any implementation begins. Trigger when the user says things like "define the interface for X", "what should the contract look like", or right after a volatility-mapping pass. Do NOT use for small changes with no new boundaries, or once implementation has already started.
---

# Contract-First Design

Define the interface/contract for each architectural boundary BEFORE
writing implementation code. The contract is what stays stable while the
implementation behind it is free to change — this is the actual isolation
mechanism, not the module split itself.

## When invoked

Use immediately after boundaries are identified (e.g. via
`volatility-mapping`), for every boundary that will be built or touched.

## Steps

For each boundary/component:

1. Define inputs and outputs precisely (types, shapes, required vs optional).
2. State invariants that must always hold true across any implementation
   behind this contract.
3. State explicitly what IS allowed to change behind this contract without
   breaking callers (this is the whole point of the boundary).
4. Flag any contract that seems likely to need to change soon itself —
   that's a signal the boundary is drawn in the wrong place, and it's worth
   revisiting the volatility mapping.

## Output format

One short spec per boundary:
```
### <BoundaryName>
Inputs: ...
Outputs: ...
Invariants: ...
Free to change internally: ...
Risk of contract churn: low/med/high — <reason if med/high>
```

## Guardrail

Keep contracts minimal. Do not add speculative fields "just in case" —
that reintroduces the coupling this skill exists to prevent. Do not write
implementation code in this skill; hand off to `pre-mortem-checklist` next.
