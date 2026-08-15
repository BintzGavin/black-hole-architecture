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

A planner is eligible when its role has no nonterminal backlog work and no prior invocation still running. It creates at most one work order and backlog entry in the same repository change. If the role has no eligible Vision gap, the planner completes as a successful no-op.

A planner may also run a recovery review when its backlog contains `blocked` or `needs_input` work and new durable resolution evidence exists. It resolves that entry, returns it to `ready`, or explicitly replaces it. It does not plan unrelated work during recovery.

An executor is eligible when its independent role backlog has one `ready` entry and no prior invocation still running. It consumes at most one work order. It must persist the claim before product changes. If no eligible work exists, the executor completes as a successful no-op.

## Parallelism and isolation

Run different roles in parallel because their write ownership is disjoint. Same-role overlap is prohibited across both phases. If one role is still active when its next tick arrives, delay or skip only that role while unrelated roles continue independently.

Each planner and executor starts from current repository state and reads the same role-local ledger:

- `.sys/plans/<role>/` for immutable task payloads and appended result evidence;
- `.sys/backlogs/<role>.md` for the authoritative execution lifecycle;
- `.sys/memory/<role>.md` for critical learnings shared across that role's runs;
- `docs/status/<ROLE>.md` for concise role progress and blocked evidence.

Other roles must not write these files. Express cross-role needs as dependencies for the owning role.

## Missed and failed ticks

- A failed planning run leaves no `ready` backlog entry, so the next executor run stops safely.
- A missed execution wave leaves its `ready` backlog entry eligible for the next execution wave.
- A planner never creates another work order while nonterminal backlog work exists for its role.
- A permission-sensitive run asks first when possible or moves to recoverable `needs_input` with the exact question preserved.
- A blocked executor records recovery evidence in its role-local backlog, work-order Result, and status before pausing.
- `blocked` and `needs_input` may return to `ready` after durable resolution; only `completed` and explicit `cancelled` are terminal.
- Restart the alternating sequence from durable role state after an interruption. Do not infer completion from elapsed time.

## What the scaffold records

Record the operating window, tick interval, phase order, phase anchor, per-role overlap rule, missed-run policy, and recovery policy in `.sys/black-hole/role-map.md`. Describe the cadence contract without generating launch configuration. The user can choose the launch mechanism separately.
