---
description: Execute the next ready task, validate it, stop at its gate
---
1. Read `.claude/tasks/INDEX.md`. Pick the first task with status `todo` whose dependencies are all `done`. If `$ARGUMENTS` names a task, use that one instead.
   - If any task is `awaiting-human`, STOP and tell me which one.
   - If the picked task is still an outline row, detail it with `_TEMPLATE.md` first, show it to me and STOP.
2. Read the task file fully. Check that every referenced decision in `docs/decisions.md` is answered. If not, STOP.
3. Set status `in-progress` in the task file and INDEX.md. Write the start time in Metrics.
4. Use plan mode thinking: list the files you will create or change. Stay inside Scope, never touch Out of scope.
5. Write the tests for the acceptance criteria first, then implement.
6. Run the validation steps in order. On failure: fix and re-run, add 1 to Rework loops. After 3 failed loops on the same step, STOP and report.
7. Visual validation: fetch Figma node screenshots with Figma MCP, render with Playwright MCP at the same viewports, save evidence to `.claude/tasks/evidence/<task>/`, list every difference you see.
8. Append to the task Log: what was done, decisions, validation results.
9. Gate:
   - `auto` and all checks green: set status `done`, write finish time, update INDEX.md, copy the task metrics row into `docs/metrics.md`, then commit following `.claude/templates/commit.md`.
   - `human`: set status `awaiting-human`, then give me a short report: what changed, test results, visual differences, and the exact Human checkpoint list. STOP.
10. Never start the next task.
