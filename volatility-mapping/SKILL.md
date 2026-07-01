---
name: volatility-mapping
description: Use when planning the architecture or decomposition of a new feature, service, or module, especially before choosing how to split code into components. Trigger on requests like "how should I structure this", "plan the architecture for X", "what's a good decomposition here", or when the user is about to start a non-trivial feature and hasn't yet decided on boundaries. Do NOT use for pure bugfixes or when boundaries are already fixed and unchangeable.
---

# Volatility Mapping

Identify architectural seams by what is likely to *change*, not by domain
nouns or function names. This is based on Volatility-Based Decomposition
(VBD) from "Righting Software" by Juval Löwy: functional/domain
decomposition looks clean today but rots as requirements shift, because
"what the system does" and "what's likely to change" are different axes.

## When invoked

Before writing any code or proposing a module/service breakdown, do this
analysis first and present it to the user for confirmation.

## Steps

1. List the things about this system/feature most likely to change
   independently over the next 6–12 months. Consider categories such as:
   - Third-party integrations
   - Business rules / pricing / policy logic
   - Data formats and schemas
   - UI / presentation
   - Storage backend or persistence strategy
   - Auth mechanisms
   - Regulatory or compliance rules

2. For each axis identified, rate:
   - Likelihood of change (low / med / high)
   - Cost of change if NOT isolated (low / med / high)

3. Propose where the boundary/interface should sit to isolate each
   high-likelihood, high-cost axis.

4. Explicitly justify each proposed boundary by the volatility it isolates,
   not by domain grouping. If a domain boundary happens to coincide with a
   volatility axis, that's fine — but state that's why, not because it's a
   noun.

## Output format

Present as a table: Axis | Likelihood | Cost if not isolated | Proposed boundary.
Follow with a one-paragraph recommended decomposition referencing the table.

## Guardrail

Do not proceed to implementation from this skill alone. Hand off to
`contract-first-design` next to define the actual interfaces.
