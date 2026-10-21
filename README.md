# Claude Code spec-driven boilerplate

Project template for building an app from a spec (PDF, markdown or Jira) and Figma designs with Claude Code.
Work runs task by task: every task is tested, visually checked against Figma, and stopped at a gate (automatic or human approval) before the next one starts.

Everything is project scoped. No global Claude config is used or changed.

## What you get

- `CLAUDE.md` rules Claude follows every session: stack, conventions, gates, validation.
- Slash commands for the whole flow: `/import-jira`, `/clarify-spec`, `/set-stack`, `/plan-tasks`, `/next-task`, `/approve-task`.
- Task system in `.claude/tasks/`: index, template, foundation task.
- MCP servers in `.mcp.json`: Figma (design data and screenshots), Atlassian (Jira import), Playwright (browser testing and screenshots).
- Decision log and metrics in `docs/`.

## Requirements

- Claude Code
- Node.js, only to run the Playwright MCP server via npx. The project itself can use any stack
- Figma account with access to the design file and a plan that allows MCP
- Atlassian account with access to the Jira project (optional, only for Jira import)

## Start a new project

1. Copy this template into a new folder (or create a repo from it).
2. Fill the `{{...}}` placeholders in `CLAUDE.md`: name, summary, Figma URL, Jira source (optional), viewports. Leave Stack for later, see Populating the tech stack below.
3. Fill the two Figma node IDs in `.claude/tasks/000-foundation.md`.
4. Add the spec to `docs/spec/`, or plan to run `/import-jira <EPIC-KEY>`.
5. Run `claude` in the folder, approve the project MCP servers when asked, then `/mcp` and authenticate `figma` (and `atlassian` if you use Jira).
6. Enable tool permissions for the sources you use, see Enabling tool permissions below.

Windows native: Playwright runs through `cmd /c npx`. On WSL, macOS or Linux change it to `"command": "npx"` in `.mcp.json`.

## Enabling tool permissions

`.claude/settings.json` ships with no tool permissions, only rules hiding the human-only docs from Claude.
Without permissions Claude asks before every Figma, Jira and Playwright call, which interrupts every task.

Ready-made rules are in `.claude/permissions/`. Each file allows the read tools and blocks the write tools:

| File | Enable when |
|------|-------------|
| `figma.json` | Always (designs come from Figma) |
| `playwright.json` | Always (visual and browser validation) |
| `jira.json` | Only if the spec comes from Jira |

Enable what you use, one of two ways:
- **Ask Claude:** `merge .claude/permissions/figma.json and .claude/permissions/playwright.json into .claude/settings.json`. Claude asks you to approve the edit.
- **Manually:** copy the `allow` and `deny` entries from each file into the matching arrays in `.claude/settings.json`.

Restart Claude Code afterwards so the rules load.
For rules only on your machine, merge into `.claude/settings.local.json` instead (gitignored).
Never allow a whole server (`mcp__figma`, `mcp__atlassian`): it includes write tools.

## Populating the tech stack

The Stack section in `CLAUDE.md` must have no `{{...}}` left before `/plan-tasks`. Two ways to fill it:

**A. `/set-stack` (recommended).** Pass only what is fixed for you, Claude suggests the rest based on the spec and Figma, checks current versions, and writes `CLAUDE.md` after you confirm.

```
/set-stack                          no preference, full suggestion
/set-stack node, react              partial, Claude fills the other layers
/set-stack python, django, htmx     any stack works, the template is not tied to Node
/set-stack nestjs, postgres, vue, vitest, playwright   everything fixed, Claude only validates and locks it
```

Reply `ok` to confirm, or change rows (`use npm instead of pnpm`).

**B. Manually.** Edit the Stack section in `CLAUDE.md` yourself:
- One line per layer: choice + major version.
- Layer not needed (e.g. no database): delete the line.
- Layer missing (e.g. ORM, message queue): add a line.
- Leave Commands and Structure as they are, task 000 fills them from the stack.

Example of a filled section:

```
## Stack (locked via /set-stack, do not add libraries without asking)
- Language + runtime: TypeScript 5, Node 22
- Framework / build tool: React 19 + Vite 7
- Styling + tokens: Tailwind 4, tokens from Figma variables in the theme config
- State / data: TanStack Query 5, mock JSON in src/data
- Unit tests: Vitest 3
- E2E tests: Playwright 1
- Lint / format: ESLint + Prettier
- Package manager: pnpm 10
```

Whatever you write here is locked: Claude will not add libraries outside it without asking.

## Workflow

```
/import-jira <EPIC-KEY>      Jira only: freeze the spec into docs/spec
/clarify-spec                answer open questions in docs/decisions.md
/set-stack <choices>         e.g. /set-stack node, react   Claude suggests the rest, you confirm
/plan-tasks                  review the task outline and detailed tasks
/clear  +  /next-task        build one task, validate, stop at the gate
/approve-task NNN approve    or: /approve-task NNN reject <feedback>
```

Repeat `/clear` + `/next-task` until every row in `.claude/tasks/INDEX.md` is done.

## Docs

- `docs/QUICKSTART.md` short setup and command reference
- `docs/WORKFLOW-EXAMPLE.md` full walkthrough of an example project
- `docs/STRUCTURE.md` every file: purpose, what to edit, template state

These three are human-only. Claude is denied read access to them in `.claude/settings.json`.

## Note

This README describes the template. Task 000 replaces it with a README for the actual app.

## License

MIT-0. Use, copy and modify freely, no attribution required. See `LICENSE`.
