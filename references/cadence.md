# Continuous cadence

Time separation is part of Black Hole Architecture. Planning and execution stay in different invocations, and a steady cadence keeps every role moving through the same two-phase loop.

## Default operating model

Use this default unless the user specifies another interval or operating window:

- Run continuously, 24/7.
- Use a one-hour system tick.
- On the planning wave, launch one eligible planner for every role in parallel.
- One hour later, launch the execution wave with one eligible executor for every role in parallel.
- Repeat indefinitely.

For each role, this is equivalent to a two-hour planning stream and a two-hour execution stream offset by one hour:

| Hour | Global phase | Per-role action |
| --- | --- | --- |
| 0 | Planning wave | Each eligible planner writes at most one role-local work order. |
| 1 | Execution wave | Each eligible executor consumes at most one role-local work order. |
| 2 | Planning wave | Each role reassesses Vision against current Reality. |
| 3 | Execution wave | Each role implements its next eligible work order. |

Continue the pattern across every hour of the operating window.

## Eligibility

A planner is eligible when its role has no ready or in-progress work order and no prior invocation still running. It creates at most one work order. If the role has no eligible Vision gap, the planner completes as a successful no-op.

An executor is eligible when its role has one ready work order and no prior invocation still running. It consumes at most one work order. If no eligible work order exists, the executor completes as a successful no-op.

## Parallelism and isolation

Run different roles in parallel because their write ownership is disjoint. Same-role overlap is prohibited across both phases. If one role is still active when its next tick arrives, delay or skip only that role while unrelated roles continue independently.

Each planner and executor starts from current repository state and reads the same role-local ledger:

- `.sys/plans/<role>/` for work orders and their state;
- `.sys/memory/<role>.md` for critical learnings shared across that role's runs;
- `docs/status/<ROLE>.md` for concise role progress and blocked evidence.

Other roles must not write these files. Express cross-role needs as dependencies for the owning role.

## Missed and failed ticks

- A failed planning run leaves no ready work order, so the next executor run stops safely.
- A missed execution wave leaves its ready work order eligible for the next execution wave.
- A planner never creates another work order while ready or in-progress work exists for its role.
- A blocked executor records evidence in its role-local work order and status before stopping when it can do so safely.
- Restart the alternating sequence from durable role state after an interruption. Do not infer completion from elapsed time.

## What the scaffold records

Record the operating window, tick interval, phase order, phase anchor, per-role overlap rule, and missed-run policy in `.sys/black-hole/role-map.md`. Describe the cadence contract without generating launch configuration. The user can choose the launch mechanism separately.
