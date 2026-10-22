# Project structure

For humans. Claude is denied read access to this file, `QUICKSTART.md` and `WORKFLOW-EXAMPLE.md` (see `.claude/settings.json`).

Legend: Edit = what you must do. Template = state of the file in the clean boilerplate copy.

## Root

| File | Purpose | Edit | Template |
|------|---------|------|----------|
| `README.md` | Describes the template (for the repo page). Task 000 replaces it with the app README | No | Keep |
| `LICENSE` | MIT-0, template license | No. Replace if a project needs a different license | Keep |
| `.gitignore` | Stack-agnostic base: secrets, local Claude settings, Playwright MCP output, OS and editor files. Evidence screenshots committed by default | Task 000 appends stack entries. Uncomment the evidence line to stop committing screenshots | Keep base only |
| `CLAUDE.md` | Rules, stack, commands, gates. Auto-loaded every session | Fill `{{...}}` before start: name, summary, Figma URL, Jira source (optional), viewports. Stack: `/set-stack` or manual edit (see README, Populating the tech stack). Task 000 fills Commands and Structure. Keep under ~100 lines | Keep with placeholders |
| `.mcp.json` | Project MCP servers: figma, atlassian, playwright | Only to add/remove a server. On WSL/Mac change playwright to `"command": "npx"` without `cmd /c` | Keep |

## .claude/

| File | Purpose | Edit | Template |
|------|---------|------|----------|
| `settings.json` | Project settings. Ships with no tool permissions, only denies Claude read access to human-only docs | Merge the files from `permissions/` you need (see README, Enabling tool permissions). Never allow a whole server | Keep empty allow |
| `permissions/figma.json` | Figma rules: 8 read tools allowed, 6 write tools denied | Merge into settings.json. Update if Figma adds tools | Keep |
| `permissions/jira.json` | Jira rules: 8 read tools allowed, 21 write/delete/admin tools denied | Merge only if spec comes from Jira | Keep |
| `permissions/playwright.json` | Playwright rule: all tools allowed (local browser) | Merge into settings.json | Keep |
| `permissions/git.json` | Git rules: status, add, commit allowed, push denied | Merge if you want a commit per finished task | Keep |
| `templates/commit.md` | Commit message format and rules used by `/next-task` and `/approve-task` | Change the format if you want. No type prefixes | Keep |
| `commands/import-jira.md` | `/import-jira KEY`: freeze Jira epic into `docs/spec/`, read only | No | Keep |
| `commands/clarify-spec.md` | `/clarify-spec`: spec + Figma to open questions, no code | No | Keep |
| `commands/set-stack.md` | `/set-stack <choices>`: your fixed choices locked, Claude suggests the rest with versions, writes Stack to CLAUDE.md after you confirm | No | Keep |
| `commands/plan-tasks.md` | `/plan-tasks`: outline in INDEX, detail next 2 to 3 tasks | No | Keep |
| `commands/next-task.md` | `/next-task [NNN]`: build one task, validate, stop at gate | No | Keep |
| `commands/approve-task.md` | `/approve-task NNN approve` or `reject <feedback>` | No | Keep |
| `tasks/INDEX.md` | Status board: gate, dependencies, status | Claude maintains. Review after `/plan-tasks`, add/split/reorder rows freely | Reset to 000 row only |
| `tasks/_TEMPLATE.md` | Shape of every task file | No | Keep |
| `tasks/000-foundation.md` | Scaffold, Figma tokens, test harness | Fill the 2 Figma node IDs. Adjust Scope if stack differs | Keep |
| `tasks/NNN-*.md` | Feature tasks created by Claude | Review each before it runs: node IDs, acceptance criteria, Out of scope | Delete |
| `tasks/evidence/` | Actual screenshots, one subfolder per task | No | Empty folder |

Changing a command file changes the process. Recurring failures fixed there are the improvements for the next version.

## docs/

| File | Purpose | Edit | Template |
|------|---------|------|----------|
| `spec/` | Requirements, source of truth. PDF/markdown or `/import-jira` output | Add the spec. Never let Claude rewrite it, changes go to decisions.md | Empty, keep README |
| `spec/README.md` | Placeholder explaining the folder | No | Keep |
| `figma/` | Reference screenshots from Figma for visual checks | No (manual exports only if Figma MCP is unavailable) | Empty folder |
| `decisions.md` | Open questions and answers. Unanswered question blocks its task | Answer questions. Add a row when a requirement changes | Reset to empty table |
| `metrics.md` | Per task time, interventions, rework loops, totals | Add failed-prompt notes per task, fill Totals at the end | Reset to empty |
| `QUICKSTART.md` | Setup steps and command order. Claude has no read access | No | Keep |
| `WORKFLOW-EXAMPLE.md` | Full walkthrough of a fake project. Claude has no read access | No | Keep |
| `STRUCTURE.md` | This file | Update when files are added or removed | Keep |

## Before first use

- `git init` and commit the clean skeleton before filling anything. That commit is the reusable template.
- Decide whether `tasks/evidence/` screenshots are committed (see `.gitignore`).
