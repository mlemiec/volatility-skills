# volatility-skills

Agent skills for planning architecture around **volatility**
instead of domain nouns, and for catching review blockers before a human
reviewer does.

Based loosely on Volatility-Based Decomposition (VBD) from Juval Löwy's
*Righting Software*: decompose around what's likely to *change*
independently, not around what a component *does*. That gives you seams
that stay stable as requirements shift, instead of a tidy-looking module
split that rots after a few sprints.

## Skills included

| Skill | Use when | Mode |
|---|---|---|
| `volatility-mapping` | Starting a new feature/service and deciding how to split it | Planning |
| `contract-first-design` | Boundaries are chosen, need to define interfaces before coding | Planning |
| `review-blockers` | Want to catch P1/P2s — either before writing code or on an existing diff | Plan mode (pre-code) or Diff mode (post-code) |
| `strangler-fig-migration` | Extracting a piece of a monolith without a big-bang rewrite | Planning |

## Recommended flow

For a new feature or component:

1. `volatility-mapping` — find the seams
2. `contract-first-design` — define stable interfaces at those seams
3. `review-blockers` (plan mode) — checklist of what review will catch
4. Implement against the checklist
5. `review-blockers` (diff mode) — self-review before opening the PR

For extracting something out of an existing monolith, run
`strangler-fig-migration` alongside step 1 to decide what to extract first
and how to cut over safely.

## Install

**Claude Code — global (all projects):**
```bash
cp -r volatility-skills/* ~/.claude/skills/
```

**Claude Code — this project only:**
```bash
cp -r volatility-skills/* .claude/skills/
```

**Via skills CLI (if published to a registry):**
```bash
npx skills add <your-namespace>/volatility-skills
```

Skills auto-trigger based on their `description` field, but you can always
invoke one explicitly, e.g. "use the volatility-mapping skill on this".

## Notes

- These are planning aids, not a substitute for actual human code review.
- `review-blockers` merges what used to be two separate skills
  (pre-mortem and self-review) into one, since their trigger conditions
  overlapped enough to confuse auto-invocation.
- PRs welcome for additional categories in `review-blockers` or additional
  migration patterns beyond strangler fig.

## License

MIT
