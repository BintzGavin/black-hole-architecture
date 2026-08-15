# Full Helios Prompt Examples

These are concrete reference implementations adapted from the Helios repository, not generic templates. They show the amount of domain context, file ownership, feature inventory, verification guidance, memory policy, and completion protocol a mature Black Hole Architecture role can carry.

## Included role pairs

### Core

- [Planner](helios-planning-core.md)
- [Executor](helios-execution-core.md)

### Studio

- [Planner](helios-planning-studio.md)
- [Executor](helios-execution-studio.md)

The four files preserve the production-scale detail of `docs/prompts/` from Helios commit [`0045419d`](https://github.com/BintzGavin/helios/tree/0045419d5020e3f3ecd5b9f320c6c38549169959/docs/prompts), with the state and boundary rules adapted to the reusable architecture in this skill.

The adaptations are intentional:

- Plans live in a role-local namespace such as `.sys/plans/core/`.
- Every role receives an independent execution backlog such as `.sys/backlogs/core.md`, rendered from the generic backlog template.
- The role backlog is authoritative for lifecycle state and shared only by that role's planner and executor.
- Each role has one memory ledger such as `.sys/memory/core.md`, shared only by that role's planner and executor.
- Status and optional generated context remain role-local.
- Cross-role needs become explicit blocked dependencies. The role never edits another role's state on its behalf.
- Global backlog, progress-ledger, and system-context writes have been removed.
- Permission questions ask first when possible and fall back to durable, recoverable `needs_input` state when unattended.
- `blocked` remains recoverable; only `completed` and explicit `cancelled` close an entry.
- Work-order identifiers are stable sequence-based names rather than dates.

## How to use them

1. Read one complete planner/executor pair after the repository's gravity map is locked.
2. Compare its level of domain specificity with the generic [planner](../assets/templates/planner.md) and [executor](../assets/templates/executor.md) templates.
3. Reproduce the depth that is justified by repository evidence: concrete feature gaps, exact paths, real commands, known dependencies, and domain-specific failure modes.
4. Replace all Helios-specific assumptions. Do not inherit its package names, ownership boundaries, commands, or product details.
5. Reapply the invariants in [architecture.md](architecture.md). The examples demonstrate prompt completeness; the generic architecture contract remains authoritative.

## What to study

- **Identity and protocol:** one role, one domain, one operating mode.
- **Boundaries:** explicit always/conditional/never rules tied to real paths.
- **Domain model:** enough concrete architecture and feature context to make decisions without inventing product intent.
- **Process:** deterministic discovery, selection, execution, verification, documentation, and presentation stages.
- **Memory:** admission rules that preserve only reusable critical learnings.
- **Completion:** observable checks and an explicit safe-stop condition.
