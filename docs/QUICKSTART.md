# Quick start

Everything is project scoped. Nothing in the user-level Claude config is used.
What each file is for and what to edit: `docs/STRUCTURE.md`.
Full walkthrough: `docs/WORKFLOW-EXAMPLE.md`.

## One-time setup
1. Open a terminal in this folder and run `claude`.
2. Claude asks once to enable the project MCP servers from `.mcp.json` (figma, atlassian, playwright). Approve the ones you use.
3. Run `/mcp`, authenticate `figma`, then `atlassian` (only if you use Jira), finishing each login in the browser.
4. Run `/mcp` again and check the servers show connected.
5. Enable tool permissions for the sources you use, see Enabling tool permissions below.
6. Spec: put a PDF/markdown in `docs/spec/`, OR set the Jira source in CLAUDE.md and run `/import-jira ABC-12`. Fill the `{{...}}` placeholders in CLAUDE.md (project name, Figma URL). The stack is filled later, see Populating the tech stack below.

## Enabling tool permissions

`.claude/settings.json` ships with no tool permissions, only rules hiding the human-only docs from Claude.
Without permissions Claude asks before every Figma, Jira and Playwright call, which interrupts every task.

Ready-made rules are in `.claude/permissions/`. Each file allows the read tools and blocks the write tools:

| File | Enable when |
|------|-------------|
| `figma.json` | Always (designs come from Figma) |
| `playwright.json` | Always (visual and browser validation) |
| `jira.json` | Only if the spec comes from Jira |
| `git.json` | Only if you want a commit per finished task (push stays manual) |

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
0. Jira only: `/import-jira <EPIC-KEY>` to freeze the spec into docs/spec.
1. `/clarify-spec` and answer the open questions in `docs/decisions.md`.
2. `/set-stack <your fixed choices>` (e.g. `/set-stack node, react`), review the table, confirm.
3. `/plan-tasks` and review the outline and detailed tasks.
4. `/next-task` runs task 000, then stops at the human gate.
5. `/approve-task 000 approve` or `/approve-task 000 reject <feedback>`.
6. `/clear`, then `/next-task` again. Repeat until INDEX.md is all done.

## Baseline
Fill `docs/metrics.md` as you go. It is the comparison for version 2.
