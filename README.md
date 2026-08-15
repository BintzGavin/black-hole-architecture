# Black Hole Architecture

A reusable agent skill for scaffolding the [Black Hole Architecture](https://agnt.one/blog/black-hole-architecture).

The skill turns one written product vision into a bounded planning and execution environment built from:

- one authoritative Vision;
- planner and executor prompt pairs;
- static, non-overlapping role ownership;
- committed work orders;
- role-local memory and status;
- explicit verification and safe idle behavior.

It is intentionally implementation-agnostic. The repository contains Markdown instructions and prompt templates, plus the minimal skill interface metadata.

## Contents

- [`SKILL.md`](SKILL.md) — operating instructions for auditing, scaffolding, and repairing the architecture.
- [`references/architecture.md`](references/architecture.md) — invariants and design model.
- [`references/template-guide.md`](references/template-guide.md) — template adaptation guidance.
- [`references/helios-examples.md`](references/helios-examples.md) — guide to two complete real-world role pairs.
- [`references/helios-planning-core.md`](references/helios-planning-core.md) and [`references/helios-execution-core.md`](references/helios-execution-core.md) — unabridged Helios Core prompts.
- [`references/helios-planning-studio.md`](references/helios-planning-studio.md) and [`references/helios-execution-studio.md`](references/helios-execution-studio.md) — unabridged Helios Studio prompts.
- [`assets/templates/planner.md`](assets/templates/planner.md) — canonical planner prompt.
- [`assets/templates/executor.md`](assets/templates/executor.md) — canonical executor prompt.
- [`assets/templates/work-order.md`](assets/templates/work-order.md) — bounded implementation contract.
- [`assets/templates/role-map.md`](assets/templates/role-map.md) — global ownership map.
- [`assets/templates/memory.md`](assets/templates/memory.md) and [`assets/templates/status.md`](assets/templates/status.md) — role-local durable state.

## Core rule

Planners may write only work orders. Executors may implement only eligible work orders and only inside their declared ownership boundaries. Shared writable surfaces must have exactly one owner.

Start with the operation selector in [`SKILL.md`](SKILL.md). For a new repository, use **Scaffold**; for an existing system, begin with **Audit**.

## Source

The architecture and prompt structure are based on the full agent-oriented edition of [Black Hole Architecture](https://agnt.one/blog/black-hole-architecture), refined against the working Helios prompt system.
