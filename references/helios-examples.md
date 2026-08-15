# Full Helios Prompt Examples

These are concrete reference implementations from the Helios repository, not generic templates. They show the amount of domain context, file ownership, feature inventory, verification guidance, memory policy, and completion protocol a mature Black Hole Architecture role can carry.

## Included role pairs

### Core

- [Planner](helios-planning-core.md) — 168 lines
- [Executor](helios-execution-core.md) — 281 lines

### Studio

- [Planner](helios-planning-studio.md) — 179 lines
- [Executor](helios-execution-studio.md) — 293 lines

The four files are unabridged snapshots of `docs/prompts/` from Helios commit [`0045419d`](https://github.com/BintzGavin/helios/tree/0045419d5020e3f3ecd5b9f320c6c38549169959/docs/prompts).

## How to use them

1. Read one complete planner/executor pair after the repository's gravity map is locked.
2. Compare its level of domain specificity with the generic [planner](../assets/templates/planner.md) and [executor](../assets/templates/executor.md) templates.
3. Reproduce the depth that is justified by repository evidence: concrete feature gaps, exact paths, real commands, known dependencies, and domain-specific failure modes.
4. Replace all Helios-specific assumptions. Do not inherit its package names, ownership boundaries, commands, product backlog, journal conventions, or shared-file practices.
5. Reapply the invariants in [architecture.md](architecture.md). The examples demonstrate prompt completeness; the generic architecture contract remains authoritative where an older example differs.

## What to study

- **Identity and protocol:** one role, one domain, one operating mode.
- **Boundaries:** explicit always/conditional/never rules tied to real paths.
- **Domain model:** enough concrete architecture and feature context to make decisions without inventing product intent.
- **Process:** deterministic discovery, selection, execution, verification, documentation, and presentation stages.
- **Memory:** admission rules that preserve only reusable critical learnings.
- **Completion:** observable checks and an explicit safe-stop condition.
