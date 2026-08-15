# Black Hole Architecture

Source: <https://agnt.one/blog/black-hole-architecture>

## Model

Black Hole Architecture replaces direct agent coordination with a repository-shaped constraint system:

- **Vision** is the written ideal state.
- **Reality** is the current project.
- **Gravity** is the gap between them.
- **Planners** turn one gap into a bounded work order.
- **Executors** implement one work order without redefining it.
- **Roles** are non-overlapping write surfaces.
- **Memory** lives in inspectable repository files.
- **Time** separates planning thought from execution thought through an alternating continuous cadence.

Agents behave like ephemeral transition functions over durable file state. No long-lived coordinator or direct agent messaging is required.

## Convergence invariants

1. One file is the authoritative Vision.
2. Vision is read-only to planner and executor roles.
3. Every role owns a distinct product surface.
4. Planners may overlap in reading, never in output namespaces.
5. Executors may overlap in activity, never in write paths.
6. Planner and executor work remain separate runs on alternating cadence ticks.
7. A planner writes intent, constraints, files, dependencies, and acceptance evidence—not implementation code.
8. An executor follows one plan and never invents adjacent work.
9. A missing eligible plan produces a successful no-op.
10. Failure leaves durable evidence that a later run can inspect.
11. Each role owns a memory ledger shared only by its planner and executor across runs.
12. Same-role invocations never overlap, and planners do not stack work over a ready or in-progress plan.

## Continuous cadence

The default system runs 24/7 on a one-hour tick. One tick launches the planning wave for all eligible roles in parallel. The next tick launches the matching execution wave. Repeating those waves gives each role a two-hour planner stream and a two-hour executor stream offset by one hour.

This cadence keeps planning and execution separate while allowing many disjoint roles to move at once. A role that has no eligible work completes as a successful no-op. A role whose prior invocation is still active waits while unrelated roles continue.

See [cadence.md](cadence.md) for the full timing, eligibility, and interruption contract.

## Role design

A role is a write boundary, not a persona or theme. Define it with:

- a concrete responsibility;
- owned files/directories;
- read-only dependencies;
- one plan namespace;
- one memory file;
- one status file;
- exact verification commands.

Shared source files must have one owner. If two roles need the same shared file, redraw the boundary, assign one owner, or have the dependent role record a dependency.

Different roles may read the same Vision, tests, API definitions, or status. Read overlap is analysis; write overlap is authority conflict.

## Role-local ledger

Use `.sys/memory/<role>.md` as the role-local memory ledger shared between that role's planner and executor. The ledger carries critical learnings across their separate runs. Other roles must not write it or use it as an informal coordination channel.

Keep the role's plan namespace and status file role-local too. Cross-role needs belong in explicit work-order dependencies so the owning role can plan them inside its own boundary.

## Planner contract

The planner answers: “What is missing in this role's Reality relative to Vision?”

It must:

- inspect Vision, Reality, existing work, memory, and status;
- choose the highest-impact eligible gap;
- keep the work finishable in one execution run;
- name exact files and measurable success criteria;
- state dependencies and forbidden changes;
- write exactly one work order and stop.

It must not edit source, tests, configuration, status, or another role's files. Avoid implementation code and excessive pseudocode; implementation judgment belongs to the executor.

## Work-order contract

A plan is a contract, not a brainstorm. It includes:

1. context, objective, trigger, and impact;
2. files to create, modify, and treat as read-only;
3. implementation constraints and public-contract implications;
4. executable verification and measurable success criteria;
5. explicit non-goals and forbidden changes;
6. dependencies and blocked conditions.

Use a stable role-local identifier instead of relying on generated dates.

## Executor contract

The executor must:

- select one eligible work order from its namespace;
- stop when none exists;
- treat the plan as the authority for scope;
- edit only listed files inside its owned surface;
- follow existing repository patterns;
- write or strengthen behavioral checks before behavior-changing code;
- run the plan checks and role checks;
- update role-local status and only genuinely reusable memory;
- record completion or blocked evidence in the work order.

The executor does not reinterpret Vision to create more work. Any out-of-scope need becomes a dependency for a later planner.

## Durable memory

Memory is a compact collection of facts that change future decisions. Good entries capture:

- a recurring domain-specific trap;
- a hidden constraint discovered from failure;
- a verification method that caught a material defect;
- an architectural fact absent from primary docs;
- a dependency pattern that repeatedly blocks work.

Do not record routine completions, generic advice, narration, or successful work with no reusable surprise.

## Anti-patterns

- competing Vision files;
- planners writing code;
- executors creating their own scope;
- shared product write paths;
- shared mutable progress files with many writers;
- a shared memory ledger used by multiple roles;
- work orders without exact files or tests;
- role names without enforceable ownership;
- direct agent-to-agent coordination;
- routine activity logs presented as memory;
- generated dates used as authoritative ordering;
- an agent that cannot safely do nothing.

## Fit

Strong candidates have testable boundaries and statically partitionable surfaces: libraries, CLIs, services, test generation, documentation synthesis, and infrastructure definitions.

Use additional safeguards or another approach when changes are hard to partition, inherently destructive, dominated by rigid data migrations, or depend on subjective visual judgment without strong feedback loops.

## Lessons from Helios

The Helios prompts validate the full sectioned pattern: identity, protocol, boundaries, philosophy, domain guide, critical memory, staged processes, verification, documentation, conflict avoidance, and final checks.

They also reveal drift worth correcting in a reusable template:

- all prompts must name the same Vision authority;
- shared backlog/context writes weaken role isolation;
- interactive “ask first” language needs a deterministic blocked state;
- date-based filenames are unreliable agent state;
- long domain-specific material belongs in direct-path prompt files;
- critical memory needs strict admission rules to avoid becoming a log.

The bundled Helios examples are adapted from the source prompts. They preserve the production-scale detail while clarifying role-local ledger ownership and replacing shared backlog and system-context writes, interactive boundary prompts, date-based plan identifiers, and global plan paths with role-local state and deterministic blocked behavior.
