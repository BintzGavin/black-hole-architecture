# Black Hole Architecture Role Map

## Gravity

**Authoritative Vision**: `{{VISION_FILE}}`

This is the sole definition of the desired state. Each role's planner and executor may read it but never edit it.

## Cadence

- **Operating window**: continuous, 24/7
- **System tick**: one hour
- **Phase order**: planning → execution → repeat
- **Planning wave**: run one eligible planner per role in parallel
- **Execution wave**: run one eligible executor per role in parallel one hour later
- **Per-role recurrence**: two-hour planner and executor streams offset by one hour
- **Overlap policy**: same-role overlap is prohibited; unrelated roles continue independently
- **No-work policy**: a role with no eligible planning or execution work completes as a successful no-op
- **Missed-run policy**: preserve the phase order for that role; never stack a new plan over ready or in-progress work

## Global invariants

- Planning and execution remain separate responsibilities.
- Every role has one plan namespace, one memory file, one status file, and one planner/executor prompt pair.
- Each memory file is a role-local memory ledger shared only by that role's planner and executor across runs; other roles must not write it.
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
- **Ledger users**: this role's planner and executor only
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
