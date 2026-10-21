---
description: Propose and lock the tech stack. Pass your fixed choices, Claude fills the rest
---
My fixed choices: `$ARGUMENTS` (may be empty or partial, e.g. `node, react` or `nestjs, postgres, vue`).

Do NOT install, scaffold or write code. Only propose, then write CLAUDE.md after I confirm.

1. Read context:
   - `CLAUDE.md` (current Stack section, project summary).
   - `docs/spec/` and `docs/decisions.md` if they contain anything (needs: backend, auth, database, SEO, realtime, offline, i18n, target browsers/devices).
   - Figma file briefly with Figma MCP: number of screens, use of variables/tokens, component library feel.
2. Treat my choices as LOCKED. Never replace them. If a choice conflicts with the spec or with another choice, flag it and ask, do not override.
3. Fill every remaining layer with ONE recommendation, preferring mainstream, stable, well documented options that work well with Claude and need the least setup:
   - Language + runtime (with version)
   - Framework / build tool
   - Styling + design token approach (tokens must map 1:1 to Figma variables)
   - UI components (or "none, custom")
   - State / data fetching
   - Backend / API (or "none" with reason)
   - Database (or "none")
   - Unit tests
   - E2E tests (default Playwright, it matches the Playwright MCP used for validation)
   - Lint / format
   - Package manager
4. Check current stable versions with web search. Never guess versions from memory.
5. Show one table and STOP:

| Layer | Choice | Version | Source | Why | Alternative |
|-------|--------|---------|--------|-----|-------------|
Source = `locked` (mine) or `suggested` (yours). Why = one line, tied to this spec or design where possible.

Then list: conflicts or risks, layers that are not needed (and why), and what I must decide.

6. Wait for my reply. I either confirm ("ok"), or change rows ("use zustand instead of redux"). Show the updated table only for changed rows, then wait again.
7. After confirmation:
   - Replace the Stack section in `CLAUDE.md` with the final list (choice + version per layer). Remove layers marked "none".
   - Add a row to `docs/decisions.md`: "Tech stack", the final stack in short form, decided by human.
   - Do not touch the Commands or Structure sections, task 000 fills those.
   - Report what was written. STOP.
