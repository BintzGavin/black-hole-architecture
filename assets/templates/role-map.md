# Black Hole Architecture Role Map

## Gravity

**Authoritative Vision**: `{{VISION_FILE}}`

This is the sole definition of the desired state. Planner and executor roles may read it but never edit it.

## Global invariants

- Planning and execution remain separate responsibilities.
- Every role has one plan namespace, one memory file, one status file, and one planner/executor prompt pair.
- Product write ownership is disjoint across roles.
- Shared writable files have exactly one owner.
- Cross-role needs are recorded as dependencies.
- Missing eligible work produces no repository changes.

## Roles

{{ROLE_SECTIONS}}

Replace `ROLE_SECTIONS` with one copy of this block per role:

```markdown
### [Role name] (`[role-id]`)

- **Responsibility**: [One sentence]
- **Domain**: `[Primary domain]`
- **Reality paths**:
  - `[Path inspected for current implementation]`
- **Owned paths**:
  - `[Exclusive product/status path]`
- **Read-only paths**:
  - `[Dependency or evidence path]`
- **Plan namespace**: `.sys/plans/[role-id]/`
- **Memory**: `.sys/memory/[role-id].md`
- **Status**: `docs/status/[ROLE-ID].md`
- **Planner prompt**: `docs/prompts/planning-[role-id].md`
- **Executor prompt**: `docs/prompts/execution-[role-id].md`
- **Vision priorities**:
  - [Promise or constraint this role evaluates]
- **Verification commands**:
  - `[Exact command]`
- **Implementation patterns**:
  - [Established pattern to preserve]
- **Documentation duties**:
  - [Role-owned truth to maintain]
- **Dependencies**:
  - [Known upstream/downstream constraint]
```

## Overlap review

For every pair of roles, compare owned paths by equality and directory-prefix containment. Record `none` only after checking both directions.

- **Conflicts found**: [None, or exact role/path pairs]
- **Resolution**: [Boundary change or single owner]
- **Approved by**: [Parent agent and, when needed, human owner]
