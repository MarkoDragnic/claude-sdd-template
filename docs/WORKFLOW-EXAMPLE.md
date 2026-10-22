# Workflow example

A complete run on a fake project, from opening the terminal to the last task.
Use it as the reference for how this setup is meant to be used.

Example project: "Book a Meeting Room" app with 3 screens (room list, room detail, booking form).
Input: spec PDF + Figma link.

`YOU` = your action. Code blocks = what you type (`>`) and what Claude answers.

---

## Phase 0: Prepare (once, about 10 min)

YOU:
1. Spec source, pick one:
   - PDF/markdown: copy it into `docs/spec/` (e.g. `docs/spec/spec.pdf`).
   - Jira: put the epic key and site URL into the Jira source line in `CLAUDE.md`. Import happens after login (step 6).
2. Open `CLAUDE.md` and fill:
   - Project name, one paragraph summary, Figma URL.
   - Leave `Stack` (filled by `/set-stack`), `Commands` and `Structure` (filled by task 000).
3. Open a terminal in this folder and start Claude:

```
> claude
```

4. Claude asks to enable the project MCP servers (figma, atlassian, playwright). Approve the ones you use.
5. Check the servers:

```
> /mcp
  figma       needs authentication
  atlassian   needs authentication
  playwright  connected
```

YOU: select `figma`, choose Authenticate, approve in the browser. Same for `atlassian` if you use Jira (skip it otherwise). Run `/mcp` again to confirm.

Enable tool permissions so Claude does not ask before every call:

```
> merge .claude/permissions/figma.json, .claude/permissions/playwright.json,
  .claude/permissions/jira.json and .claude/permissions/git.json into .claude/settings.json
```

Approve the edit, then restart Claude Code (`/exit`, then `claude`). Skip `jira.json` if your spec is a PDF, skip `git.json` if you do not want a commit per task.
Read tools now run without prompts, write tools are blocked.

6. Jira only, freeze the spec:

```
> /import-jira ROOM-1
```

```
Exported 14 issues to docs/spec/jira-ROOM-1.md
Figma links found: 3 (ROOM-4, ROOM-7, ROOM-9)
Attachments downloaded: 3 (docs/spec/attachments/)
Unreadable: none
```

YOU: open the file, check nothing important is missing, and check the attachment descriptions match the images. From here on Claude works from this file, never live Jira.

Start a stopwatch. That is your baseline time.

---

## Phase 0b: Figma only, no spec (about 30 min)

Skip this if you got a spec or Jira tickets.

```
> /clear
> /write-spec https://www.figma.com/design/abc123/Room-Booking
```

```
Screens found: 7 (4 desktop, 3 mobile), components page and 31 variables noted
Screenshots saved: docs/figma/ (7 files), inventory: docs/figma/inventory.md
Written: docs/spec/brief.md, docs/spec/stories.md (US-01 to US-05)
Assumptions: 6, open questions: 8 (docs/decisions.md)
```

YOU: this is a human gate, the most important review of the whole run.
- Read `brief.md`: is the product understood correctly?
- Read every story: acceptance criteria, states, out of scope. Edit the files directly.
- Every ASSUMPTION: keep it, change it, or delete it.
- Answer the open questions in `docs/decisions.md`.

The reviewed brief and stories are now the spec. Continue with Phase 1, which finds only what is still unclear.

---

## Phase 1: Clarify the spec (about 20 to 30 min)

```
> /clarify-spec
```

Claude reads the spec and Figma, writes no code, and stops:

```
Summary: internal app for booking meeting rooms...

Screen inventory:
| Frame           | Node    | Spec section | Note                 |
| Room list       | 12:340  | 2.1          | ok                   |
| Room detail     | 12:512  | 2.2          | ok                   |
| Booking form    | 14:88   | 2.3          | ok                   |
| Booking success | 14:201  | none         | no spec              |
| Room list mob.  | missing | 2.1          | no mobile design     |

Open questions written to docs/decisions.md:
1. Can a user book overlapping slots in different rooms?
2. Spec says max 2h booking, form allows 4h. Which wins?
3. No error state design for a failed booking.
4. No mobile design for room list.
5. Data source: mock JSON or real API?
Waiting for answers.
```

YOU: answer every question. Either edit `docs/decisions.md` directly, or reply:

```
> 1 yes. 2 spec wins, 2h max. 3 inline red message under submit button,
  use error color token. 4 stack cards vertically, 16px gutter.
  5 mock JSON in src/data, no backend. Update decisions.md and close them.
```

This is the highest value step. Every unanswered question becomes a guess later.

---

## Phase 1b: Set the stack (about 5 to 10 min)

Pass what is fixed for you. Claude fills the rest, using the spec and Figma as input.

```
> /clear
> /set-stack node, react
```

```
| Layer             | Choice            | Version | Source    | Why                                  | Alternative   |
| Language/runtime  | TypeScript, Node  | 5.x/22  | locked    | your choice                          |               |
| Framework/build   | React + Vite      | 19 / 7  | locked/sug| SPA, no SEO need in spec             | Next.js       |
| Styling + tokens  | Tailwind          | 4.x     | suggested | Figma variables map to theme config  | CSS modules   |
| UI components     | none, custom      |         | suggested | 3 screens, custom design             | shadcn/ui     |
| State / data      | TanStack Query    | 5.x     | suggested | mock JSON now, API later (dec. #5)   | plain fetch   |
| Backend / API     | none              |         | suggested | decision #5: mock JSON, no backend   |               |
| Unit tests        | Vitest            | 3.x     | suggested | native to Vite                       | Jest          |
| E2E tests         | Playwright        | 1.x     | suggested | same tool as validation MCP          | Cypress       |
| Lint / format     | ESLint + Prettier |         | suggested | standard                             | Biome         |
| Package manager   | pnpm              | 10.x    | suggested | fast, strict                         | npm           |

Not needed: database (no persistence in spec).
Decide: confirm, or change rows.
```

(Versions above are illustrative. Claude checks current versions with web search.)

YOU: confirm, or change rows:

```
> use npm instead of pnpm, rest ok
```

Claude writes the Stack section in `CLAUDE.md` and logs the decision in `docs/decisions.md`.

You can also run `/set-stack` with no arguments to get a full suggestion, or with everything fixed (`/set-stack nestjs, postgres, vue, vitest`) to just validate and lock it.

---

## Phase 2: Plan tasks (about 15 min)

```
> /clear
> /plan-tasks
```

```
INDEX.md updated:
| 000 | Foundation                        | human | -   | todo |
| 001 | Room list (desktop + mobile)      | human | 000 | todo |
| 002 | Room data layer + filtering logic | auto  | 000 | todo |
| 003 | Room detail                       | human | 001 | todo |
| 004 | Booking form + validation         | human | 003 | todo |
| 005 | Booking success + error states    | human | 004 | todo |

Detailed: 001, 002. Others are outline rows only.
```

YOU: open the detailed task files in your editor and check:
- Figma node IDs point to the right frames.
- Acceptance criteria are testable and complete.
- Out of scope stops Claude from building the next task.

Edit the markdown directly if something is wrong. No need to ask Claude.

---

## Phase 3: Build loop

Pattern for every task:

```
> /clear
> /next-task            (or /next-task 002 to pick a specific task)
```

Then, for human gate tasks:

```
> /approve-task <task> approve
> /approve-task <task> reject <precise feedback>
```

### Task 000, human gate

```
> /clear
> /next-task
```

```
Task 000 -> awaiting-human

Changed: Vite + React scaffold, src/styles/tokens.css (24 tokens),
Layout.tsx, Vitest + Playwright configured, CLAUDE.md commands filled.

Tests: unit 1/1 ok, e2e 2/2 ok, lint ok, typecheck ok

Token report:
| Token         | Code    | Figma               |
| color-primary | #2F5BEA | Primary/500 #2F5BEA |
| space-4       | 16px    | Spacing/4 16        |

Screenshots: .claude/tasks/evidence/000/

Please confirm:
- Stack and folder structure acceptable
- Token table matches Figma
- Shell screenshots look right
```

YOU: open the screenshots, scan the token table, run the dev server and look at it for 30 seconds. Then:

```
> /approve-task 000 approve
```

or, if something is wrong:

```
> /approve-task 000 reject font family is Inter, Figma uses Plus Jakarta Sans. fix the token
```

On reject Claude fixes only that, validates again and stops at the gate again.
Human interventions and rework loops are counted automatically.

### Task 002, auto gate

```
> /clear
> /next-task 002
```

```
Task 002 -> done
Tests 9/9, all acceptance criteria covered. Logged, metrics copied.
Committed a3f91c2: Task 002: Room data layer + filtering logic
```

Nothing to do. Pure logic covered by tests closes itself.

### Task 001, human gate with visual check

```
> /clear
> /next-task 001
```

```
Task 001 -> awaiting-human

Visual differences vs Figma (1440):
- Card shadow lighter than Figma
- Room capacity icon missing (custom icon, not in assets)
Mobile (375): matches decision #4.

Please confirm: visual match, card hover state, empty list message.
```

YOU: reject with exact direction:

```
> /approve-task 001 reject export capacity icon from Figma node 12:377 as SVG, card shadow use shadow-lg token
```

When it comes back clean:

```
> /approve-task 001 approve
```

```
Task 001 -> done
Committed 7b20e4d: Task 001: Room list (desktop + mobile)
```

Commits happen only if `git.json` permissions are merged and the folder is a git repo. Claude never pushes: review `git log` and push yourself.

### Outline-only task

When `/next-task` reaches a task that is only a row in INDEX.md (e.g. 003), Claude first writes the full task file, shows it and stops.

YOU: review it (node IDs, acceptance criteria, scope), edit if needed, then:

```
> /clear
> /next-task 003
```

### Claude stops after 3 failed loops

Claude reports what keeps failing. Do not answer "try again". Usually one of:
- Task too big: split it into two task files and update INDEX.md.
- Missing decision: add it to `docs/decisions.md`, answer it, run the task again.
- Wrong acceptance criterion: fix it in the task file.

---

## Roles

| YOU | CLAUDE |
|-----|--------|
| Answer spec questions | Find gaps, contradictions, missing states |
| Review task files | Write task files |
| Judge visuals at human gates | Build, test, screenshot, list differences |
| Approve or reject with precise feedback | Fix only what feedback says |
| `/clear` before every task | Track status, log, metrics |
| Note what failed or annoyed you | |

## Rules

- Always `/clear` before `/next-task`. One task per fresh context.
- Rejections must say what, where and expected value. "Looks wrong" wastes a loop.
- Requirement changed? Update `docs/decisions.md` first, then the task file, then run.
- Check `docs/metrics.md` after each task. Add notes on failed prompts while you still remember them.

## Finish

All rows in INDEX.md are `done`.
Fill the Totals block in `docs/metrics.md`, especially "What I would change in version 2".
Version 2 = same project, new techniques, compared against these numbers.
