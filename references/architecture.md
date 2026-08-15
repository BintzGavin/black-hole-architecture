# Black Hole Architecture

Source: <https://agnt.one/blog/black-hole-architecture>

## Model

Black Hole Architecture replaces direct agent coordination with a repository-shaped constraint system:

- **Vision** is the written ideal state.
- **Reality** is the current project.
- **Gravity** is the gap between them.
- **Planners** turn one gap into a bounded work order.
- **Executors** implement one work order without redefining it.
- **Backlogs** give every role an independent, authoritative execution ledger.
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
9. A missing eligible backlog entry produces a successful no-op.
10. Failure leaves durable evidence that a later run can inspect.
11. Each role owns a memory ledger shared only by its planner and executor across runs.
12. Same-role invocations never overlap, and planners do not stack work over nonterminal backlog entries.
13. Every role has one independent backlog shared only by that role's planner and executor.
14. Executors persist `ready` → `in_progress` claims before product changes.
15. Permission-sensitive work remains recoverable as `needs_input`; dependency-blocked work remains recoverable as `blocked`.
16. Only `completed` and explicit `cancelled` backlog entries are terminal.

## Continuous cadence

The default system runs 24/7 on a one-hour tick. One tick launches the planning wave for all eligible roles in parallel. The next tick launches the matching execution wave. Repeating those waves gives each role a two-hour planner stream and a two-hour executor stream offset by one hour.

This cadence keeps planning and execution separate while allowing many disjoint roles to move at once. A role that has no eligible `ready` backlog entry completes as a successful no-op. A role whose prior invocation is still active waits while unrelated roles continue.

See [cadence.md](cadence.md) for the full timing, eligibility, and interruption contract.

## Role design

A role is a write boundary, not a persona or theme. Define it with:

- a concrete responsibility;
- owned files/directories;
- read-only dependencies;
- one plan namespace;
- one independent execution backlog;
- one memory file;
- one status file;
- exact verification commands.

Shared source files must have one owner. If two roles need the same shared file, redraw the boundary, assign one owner, or have the dependent role record a dependency.

Different roles may read the same Vision, tests, API definitions, or status. Read overlap is analysis; write overlap is authority conflict.

## Role-local ledger

Use `.sys/memory/<role>.md` as the role-local memory ledger shared between that role's planner and executor. The ledger carries critical learnings across their separate runs. Other roles must not write it or use it as an informal coordination channel.

Use `.sys/backlogs/<role>.md` as a separate role-local execution ledger rendered from the generic backlog template. It is authoritative for work lifecycle and shared only between the same role's planner and executor. Keep the role's plan namespace and status file role-local too. Cross-role needs belong in explicit work-order dependencies so the owning role can plan them inside its own boundary.

The backlog prevents a completed plan from becoming eligible again and makes skipped or stranded work visible. It does not replace the same-role non-overlap rule. The executor must persist its claim in current durable repository state before modifying product files.

## Recoverable intervention

Ask first when a human decision could authorize safe continuation. If an interactive answer is unavailable, preserve the exact question in the role backlog as `needs_input` and pause. Do not convert an unanswered permission question into a terminal result.

Use `blocked` for missing dependencies or other recoverable conditions. A human or authorized planner may return `needs_input` or `blocked` to `ready` only after recording durable resolution evidence. The planner may run a recovery review for this purpose but must not stack unrelated work. If the resolution changes scope or ownership, enqueue a replacement work order before explicitly cancelling the old one.

Only `completed` and explicit `cancelled` are terminal states.

## Planner contract

The planner answers: “What is missing in this role's Reality relative to Vision?”

It must:

- inspect Vision, Reality, existing work, memory, and status;
- choose the highest-impact eligible gap;
- keep the work finishable in one execution run;
- name exact files and measurable success criteria;
- state dependencies and forbidden changes;
- write exactly one work order and stop;
- append its role-local backlog entry in the same repository change.

It must not edit source, tests, configuration, status, or another role's files. Avoid implementation code and excessive pseudocode; implementation judgment belongs to the executor.

## Work-order contract

A plan is a contract, not a brainstorm. It includes:

1. context, objective, trigger, and impact;
2. files to create, modify, and treat as read-only;
3. implementation constraints and public-contract implications;
4. executable verification and measurable success criteria;
5. explicit non-goals and forbidden changes;
6. dependencies and blocked conditions.

Use a stable role-local identifier instead of relying on generated dates. The work order becomes the immutable task payload when its backlog entry becomes `ready`; it does not own lifecycle status. A `needs_input` draft may change only to apply its recorded resolution. Its role-local backlog entry is the authoritative state.

## Executor contract

The executor must:

- select one eligible `ready` entry from its independent role backlog;
- persist the `in_progress` claim before product edits;
- stop when none exists;
- treat the plan as the authority for scope;
- edit only listed files inside its owned surface;
- follow existing repository patterns;
- write or strengthen behavioral checks before behavior-changing code;
- run the plan checks and role checks;
- update role-local status and only genuinely reusable memory;
- record lifecycle state in the backlog and completion, input, or blocked evidence in the work order Result.

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
- a global execution backlog used by multiple roles;
- a shared memory ledger used by multiple roles;
- lifecycle state duplicated across the backlog and work-order header;
- unanswered permission questions converted into terminal blocked work;
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
- global backlog or context writes weaken role isolation;
- interactive “ask first” language needs a durable `needs_input` fallback for unattended runs;
- date-based filenames are unreliable agent state;
- long domain-specific material belongs in direct-path prompt files;
- critical memory needs strict admission rules to avoid becoming a log.

The bundled Helios examples are adapted from the source prompts. They preserve the production-scale detail while clarifying role-local ledger ownership and replacing global backlog and system-context writes, ephemeral permission questions, date-based plan identifiers, and global plan paths with independent role state and recoverable input requests.
