---
name: roll-out-black-hole-architecture
description: >-
  Design, audit, scaffold, validate, and repair repository-native Black Hole
  Architecture systems. Use when a project needs vision-driven planner and
  executor roles, non-overlapping file ownership, committed work orders,
  role-local memory, or reusable Helios-style agent prompt pairs.
---

# Roll Out Black Hole Architecture

Create a repository environment where stateless roles converge on one written Vision through strict planner/executor separation and disjoint write ownership.

Read [references/architecture.md](references/architecture.md) before defining roles. Read [references/template-guide.md](references/template-guide.md) before copying templates.

## Select the operation

- **Audit**: inspect the current architecture and report drift without editing.
- **Scaffold**: discover the repository, agree a role map, copy the Markdown templates, resolve them, and validate the result.
- **Repair**: inspect an existing setup, make only requested corrections, and revalidate.
- **Template**: provide or adapt one bundled prompt without scaffolding a repository.

Do not turn an audit or explanation request into repository writes.

## 1. Inspect the repository

Read repository instructions, primary product documentation, workspace/package structure, tests, current prompt files, work orders, status, and durable memory. Use history only when it clarifies ownership or recurring failures.

Identify:

1. exactly one authoritative Vision file;
2. the Reality surfaces implementing each part of that Vision;
3. role boundaries defined by non-overlapping write paths;
4. read-only dependencies between roles;
5. exact verification commands;
6. shared files that need one owner or role-local replacements.

### Optional read-only scout fanout

For a broad repository, use read-only scouts when subagents are available:

- one scout maps Vision and governance;
- one or more scouts map independent package/domain ownership;
- one scout maps tests and verification commands;
- one scout maps existing prompts, plans, memory, and status.

Every scout must not edit, create setup files, or choose the final architecture. Give each scout a bounded path or concern. The parent agent must synthesize their evidence, resolve contradictions, validate non-overlap, and own the final gravity map.

Do not fan out when one focused inspection can establish the map more cheaply.

## 2. Review and lock the gravity map

Copy [assets/templates/role-map.md](assets/templates/role-map.md) into a draft and fill it with:

- the single Vision path;
- role names and responsibilities;
- owned paths;
- read-only paths and dependencies;
- Reality paths;
- verification commands;
- plan, memory, status, and prompt paths.

Reject roles that are merely topics. A valid role is a static write boundary. Treat path equality and directory-prefix containment as overlap. If two plausible maps produce different ownership, ask the user to choose after showing the conflict.

Do not create repository files until the parent agent has locked a map with zero write overlap.

## 3. Scaffold the Markdown files

Create these paths:

```text
.sys/black-hole/role-map.md
.sys/black-hole/work-order-template.md
.sys/plans/<role>/
.sys/memory/<role>.md
docs/prompts/planning-<role>.md
docs/prompts/execution-<role>.md
docs/status/<ROLE>.md
```

Copy and resolve:

1. `assets/templates/role-map.md` → `.sys/black-hole/role-map.md`
2. `assets/templates/work-order.md` → `.sys/black-hole/work-order-template.md`
3. For each role, `assets/templates/planner.md` → `docs/prompts/planning-<role>.md`
4. For each role, `assets/templates/executor.md` → `docs/prompts/execution-<role>.md`
5. For each role, `assets/templates/memory.md` → `.sys/memory/<role>.md`
6. For each role, `assets/templates/status.md` → `docs/status/<ROLE>.md`
7. Create the role's empty `.sys/plans/<role>/` namespace for future work orders.

Replace every `{{PLACEHOLDER}}` with repository evidence. Render empty optional lists as `- None declared.` rather than deleting sections. Preserve the major section order in planner and executor prompts.

### Optional role-builder fanout

After the parent agent locks `.sys/black-hole/role-map.md`, it may assign one role builder per disjoint role.

Each role builder receives the approved role-map section and the five source templates it needs. Role builders may create only their disjoint role-local files:

- their planner prompt;
- their executor prompt;
- their memory file;
- their status file;
- their plan directory.

Role builders must not edit shared files, another role's files, product code, or the approved ownership map. The parent agent creates the two shared files, checks all builder outputs, and resolves global consistency.

## 4. Review each prompt pair

Confirm:

- The planner reads Vision, Reality, existing plans, memory, and status; chooses one gap; writes one work order; and never changes product files.
- The executor locates one eligible work order; never invents scope; edits only owned paths; verifies success; and updates only role-local state.
- Both prompts name the same Vision, plan, memory, status, and ownership paths.
- Cross-role needs become dependencies, not unauthorized edits.
- Memory accepts only critical reusable learnings, never routine activity.
- Domain detail extends the common contract without weakening it.

## 5. Validate manually

Treat any failed check as an invalid architecture:

1. There is no unresolved `{{PLACEHOLDER}}` in any copied file.
2. Every prompt names the one approved Vision.
3. No two roles own the same path or ancestor/descendant paths.
4. Every shared writable file has exactly one owner.
5. Planner output is limited to its plan namespace.
6. Executor writes are limited to its work order, owned product paths, memory, and status.
7. The work-order template contains context, exact files, implementation constraints, tests, non-goals, and dependencies.
8. Every verification command is real for this repository.
9. Every Reality and read-only path exists or is explicitly expected to be created by the Vision.
10. Planner and executor prompts agree on how plans become `ready`, `completed`, or `blocked`.
11. A planner with no eligible gap and an executor with no eligible work both stop without changes.

Search the copied files directly for placeholder delimiters and compare the final role map against every prompt. Do not rely on self-report from role builders.

## 6. Repair an existing setup

Audit in this order:

1. competing Vision authorities;
2. ownership overlap;
3. planner/executor boundary erosion;
4. contradictory path references;
5. vague or oversized work orders;
6. missing verification evidence;
7. routine logs polluting critical memory;
8. prompts that cannot safely no-op.

Repair the smallest structural cause. Do not paper over shared ownership with coordination prose.

## 7. Report

State:

- the selected Vision;
- the final role/ownership map;
- Markdown files created or repaired;
- validation results;
- any unresolved cross-role dependency or human decision.

Do not claim convergence from prompt structure alone. The proof is repeated bounded plans, scoped implementations, passing checks, and safe idle behavior.
