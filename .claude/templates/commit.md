# Commit message format

Used by `/next-task` (auto gate) and `/approve-task approve`. Replace every `{{...}}`, keep the layout.
No type prefixes (no "feat:", "fix:"). Subject max 72 characters.

```
Task {{NNN}}: {{task title}}

{{One line: what was built}}

Acceptance criteria: {{met}}/{{total}} met
Tests: unit {{passed}}/{{total}}, e2e {{passed}}/{{total}}
Gate: {{auto | human approved}}
Rework loops: {{n}}
```

## Commit rules
1. Commit only after the task status is `done` and metrics are written, so they are part of the commit.
2. If the folder is not a git repository, or there is nothing to commit, skip and say so.
3. Stage with `git add -A` (`.gitignore` decides what is excluded). Check `git status` first and STOP if anything looks like a secret (`.env`, keys, tokens).
4. One commit per task. Never amend, rebase or force anything.
5. Never push. Pushing is always done by the human.
6. Report the commit hash and subject.
