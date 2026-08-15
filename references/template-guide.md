# Prompt template guide

The bundled Markdown templates are source assets for committed role prompts and repository state. Copy them to the project's chosen paths and replace placeholders with repository evidence.

## Files

- `assets/templates/planner.md`: full vision-driven planner contract.
- `assets/templates/executor.md`: full plan executor and self-documenter contract.
- `assets/templates/work-order.md`: exact structure every planner-created work order follows.
- `assets/templates/memory.md`: bounded critical-learning store.
- `assets/templates/status.md`: concise role state and evidence.
- `assets/templates/role-map.md`: the approved ownership, path, ledger, and cadence contract.

## Placeholder groups

### Identity

- `{{ROLE_NAME_UPPER}}`: human-readable role name in uppercase.
- `{{ROLE_ID}}`: stable lowercase role identifier.
- `{{DOMAIN}}`: compact domain label or primary directory.
- `{{RESPONSIBILITY}}`: one-sentence role purpose.

### Gravity and state

- `{{VISION_FILE}}`: sole authoritative Vision path.
- `{{PLAN_DIR}}`: role-local work-order directory.
- `{{MEMORY_FILE}}`: role-local critical-learning file.
- `{{STATUS_FILE}}`: role-local status file.
- `{{WORK_ORDER_TEMPLATE}}`: shared read-only work-order template.
- `{{ROLE_SECTIONS}}`: one completed role-map section per role.

### Scope

- `{{OWNED_PATHS}}`: exclusive role write paths.
- `{{OTHER_ROLE_PATHS}}`: product paths owned by peers.
- `{{READ_ONLY_PATHS}}`: dependencies the role may inspect but never edit.
- `{{REALITY_PATHS}}`: implementation surfaces the planner compares with Vision.

### Domain detail

- `{{VISION_PRIORITIES}}`: concrete promises or constraints to evaluate.
- `{{VERIFICATION_COMMANDS}}`: exact project commands.
- `{{IMPLEMENTATION_PATTERNS}}`: established patterns worth preserving.
- `{{DOCUMENTATION_TASKS}}`: role-local truth that changes with implementation.
- `{{DEPENDENCIES}}`: known interfaces or upstream/downstream constraints.

List placeholders become Markdown bullets. Empty optional lists become an explicit “None declared” item so omissions remain visible.

## Adaptation rules

1. Preserve the major section order. Agents benefit from a stable operational grammar across roles.
2. Replace every placeholder with repository evidence. Never leave aspirational commands or paths.
3. Keep shared contract language identical across roles; put domain knowledge in the dedicated domain sections.
4. Treat planner and executor prompts as a matched pair with the same paths and responsibility.
5. Keep the planner implementation-light. Move coding judgment to the executor.
6. Assign every writable status, progress, context, or memory file to one role.
7. Use one role-local memory ledger for the planner and executor of that role. Other roles must not write it.
8. Preserve the cadence block and change its default only when the user specifies another interval or operating window.
9. Prefer direct paths over repeating large guidance in a parent instruction.
10. Validate manually after every prompt edit.

## Manual reuse

Copy the relevant asset to the project's prompt directory, replace placeholders, and apply the architectural checklist in `SKILL.md`. Do not shorten away boundaries, critical-memory admission rules, plan selection, verification, or final checks.
