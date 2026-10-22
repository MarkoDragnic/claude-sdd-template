---
description: Record human approval or rejection for an awaiting-human task
---
Arguments: `$ARGUMENTS` (format: `<task> approve` or `<task> reject <feedback>`).

- approve: set the task status to `done` in the task file and INDEX.md, write the finish time, add 1 to Human interventions, log "Approved by human". Copy the task metrics row into `docs/metrics.md`. Then commit following `.claude/templates/commit.md` (Gate: human approved).
- reject: keep the status `in-progress`, add 1 to Human interventions and 1 to Rework loops, log the feedback verbatim, then fix ONLY what the feedback asks, re-run validation and stop at the gate again. Do not commit.
