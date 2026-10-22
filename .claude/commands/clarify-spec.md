---
description: Read the spec and Figma, list ambiguities and gaps before any planning
---
If `docs/spec/` has no spec files (README.md does not count): if CLAUDE.md has a Jira source, tell me to run `/import-jira`, otherwise tell me to run `/write-spec`. Then STOP.

If `docs/decisions.md` already has questions, do not repeat them. Only add what is still unclear.
If `docs/figma/inventory.md` exists, reuse it and the saved screenshots instead of calling Figma again.

Read everything in `docs/spec/` and browse the Figma file from CLAUDE.md with Figma MCP (pages, frames, variables). Do NOT write code and do NOT plan tasks yet.

Produce:
1. A short summary of the product as you understand it (max 10 lines).
2. Screen inventory: every Figma frame you found, its node ID, and which spec section it maps to. Flag screens with no spec and spec features with no screen.
3. Ambiguities, contradictions, missing edge cases, missing states (empty, loading, error), missing responsive designs.
4. Assumptions you would make if nobody answered.

Write items 3 and 4 into `docs/decisions.md` as open questions, numbered. Then STOP and wait for my answers.
