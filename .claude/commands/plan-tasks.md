---
description: Turn the clarified spec into a task outline and fully detail the next tasks
---
Preconditions:
- The Stack section in `CLAUDE.md` has no `{{...}}` placeholders. If it does, tell me to run `/set-stack` or fill it manually, and STOP.
- `docs/decisions.md` has no open questions that affect planning. If it does, list them and STOP.

1. Create the full task outline in `.claude/tasks/INDEX.md`: one row per task, vertical slices (one feature end to end per task), with gate and dependencies. Task 000 already exists, start features at 001.
2. Gate rule: `human` for anything visual, UX, layout or architecture. `auto` only for pure logic fully covered by tests.
3. Fully detail ONLY the next 2 to 3 tasks using `.claude/tasks/_TEMPLATE.md`, with real Figma node IDs, numbered acceptance criteria and test scenarios covering happy path, edge cases and error states.
4. Leave other tasks as outline rows only.

Show me the outline and the detailed tasks, then STOP for review.
$ARGUMENTS
