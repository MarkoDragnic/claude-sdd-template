# Project: {{PROJECT_NAME}}

## What this is
{{ONE_PARAGRAPH_SUMMARY_OF_THE_PRODUCT}}

Source of truth:
- Spec: `docs/spec/` (requirements). If code and spec disagree, the spec wins. If the spec is unclear, STOP and ask.
- Jira source (optional): {{EPIC_KEY or JQL}} on {{https://yourcompany.atlassian.net}}. Imported into `docs/spec/` with `/import-jira`. Never read live Jira during tasks, never write to Jira.
- Design: Figma file {{FIGMA_FILE_URL}}. Node IDs per screen are listed in each task.
- Decisions made during clarification: `docs/decisions.md`

## Stack (locked via /set-stack, do not add libraries without asking)
- Language + runtime: {{...}}
- Framework / build tool: {{...}}
- Styling + tokens: {{...}}
- UI components: {{...}}
- State / data: {{...}}
- Backend / API: {{...}}
- Database: {{...}}
- Unit tests: {{...}}
- E2E tests: {{...}}
- Lint / format: {{...}}
- Package manager: {{...}}

## Commands
- Install: `{{pnpm install}}`
- Dev server: `{{pnpm dev}}` (runs on {{http://localhost:5173}})
- Unit tests: `{{pnpm test}}`
- E2E tests: `{{pnpm test:e2e}}`
- Lint + typecheck: `{{pnpm lint && pnpm typecheck}}`
- Build: `{{pnpm build}}`

## Structure
{{fill after task 000, keep to 10 lines}}

## Conventions
- Use design tokens only. No hardcoded colors, font sizes, spacing or radii.
- Reuse components from `{{src/components}}` before creating new ones.
- One component per file. Name files {{PascalCase.tsx}}.
- Every acceptance criterion in a task maps to at least one test.

## Task workflow (mandatory)
- Tasks live in `.claude/tasks/`. Index and status: `.claude/tasks/INDEX.md`.
- Work on exactly ONE task per session. Never start the next task on your own.
- Never touch anything listed under "Out of scope" in the current task.
- Execute tasks only through `/next-task`. Human approval only through `/approve-task`.
- A task is done only when every validation step passes AND its gate is satisfied.
- Gate `auto`: you may mark done when all checks pass.
- Gate `human`: stop, present evidence, set status `awaiting-human`. Do not mark done yourself.

## Visual validation rules
- Viewports: {{1440x900}} desktop, {{375x812}} mobile.
- Get the reference with Figma MCP (screenshot of the task's node ID), render the page with Playwright MCP at the same viewport.
- Report differences in layout, spacing, typography, color and missing elements. Never claim "pixel perfect".
- Save screenshots to `docs/figma/` (reference) and `.claude/tasks/evidence/{{task}}/` (actual).

## Never
- Never skip, weaken or delete a failing test to make it pass.
- Never mark a `human` gate task as done.
- Never invent requirements. Ambiguity goes to `docs/decisions.md` as an open question and you STOP.
- Never commit secrets or tokens.
- Never call write tools on Figma or Jira (create, edit, transition, comment, write to canvas). Both are read-only sources.
