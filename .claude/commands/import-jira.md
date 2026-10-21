---
description: Freeze a Jira epic (or JQL) into docs/spec as markdown, read only
---
Source: `$ARGUMENTS` (an epic key like `ABC-12`, or a JQL query). If empty, use the Jira source from CLAUDE.md. If that is empty too, ask me and STOP.

Use the Atlassian MCP. READ ONLY: never create, edit, transition or comment on Jira issues.

1. Fetch the epic and every child issue (stories, tasks, bugs, subtasks).
2. Write `docs/spec/jira-<KEY>.md` with:
   - Header: source key or JQL, Jira site, export date and time.
   - Epic: summary, description.
   - One section per issue, ordered by rank: key, type, summary, status, description, acceptance criteria (verbatim), linked issues, attachment names, and comments that change or clarify requirements.
   - Figma links found anywhere in the issues, listed with the issue key they came from.
3. Keep the wording verbatim. Do not summarise, merge or interpret requirements.
4. Download image and document attachments that carry requirements (mockups, screenshots, flows) to `docs/spec/attachments/<ISSUE-KEY>/`, view them, and describe what they show in the issue section with a link to the file.
5. List anything you could not read or download at the end of the file.
6. Report: number of issues exported, Figma links found, attachments downloaded, unreadable items. STOP.

From now on `docs/spec/` is the source of truth, not live Jira. Re-run this command only when I ask to refresh the spec.
