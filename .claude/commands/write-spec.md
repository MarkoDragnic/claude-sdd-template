---
description: Write the product brief and user stories from the Figma file when no spec is given
---
Figma source: `$ARGUMENTS` (a Figma link). If empty, use the Figma file from CLAUDE.md. If that is empty too, ask me and STOP.

Do NOT write code, plan tasks or pick a stack.

Figma calls are rate limited, so:
- Start with `get_metadata` on the file to get pages and frames.
- Take ONE screenshot per screen frame with `get_screenshot`, save it to `docs/figma/<frame-name>.png`. Reuse saved files, never fetch the same screenshot twice.
- Use `get_design_context` only on specific nodes where behavior is unclear from the screenshot.
- Write `docs/figma/inventory.md`: page, frame name, node ID, desktop or mobile, screenshot file.

Steps:
1. Inventory: list pages and screen frames. Skip the cover page. Note the components page and variables, do not write stories for them. Pair desktop and mobile variants. Group screens into user flows (e.g. list, detail, form, success).
2. Write `docs/spec/brief.md`:
   - Product summary (max 10 lines): what it is, who uses it, main goal.
   - Users and their goals.
   - Feature list.
   - Screen flow: how screens connect (links, buttons, navigation).
   - Global rules: viewports found in the design, design system notes (variables, components).
   - Not designed: things a real product needs but Figma does not show (auth, errors, empty states, etc.).
   - Assumptions, each marked ASSUMPTION.
3. Write `docs/spec/stories.md`: one story per screen or feature, IDs US-01, US-02..., using `.claude/templates/story.md`.
4. Rules for content:
   - Describe what is visible: fields, buttons, labels, content, navigation, states.
   - Logical consequences are allowed (a form submit needs validation) but marked ASSUMPTION.
   - Never invent business rules, data sources, roles or integrations. Those become open questions.
5. Add open questions to `docs/decisions.md`, numbered: unclear interactions, missing states, validation rules, data source, auth, responsive behavior without a mobile design. Reference them from the stories.
6. Report: screens found, stories written, number of assumptions, open questions. STOP.

The brief and stories are a draft for human review. They become the spec only after I review and edit them.
