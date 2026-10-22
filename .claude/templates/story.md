# User story format

Used by `/write-spec`. One story per screen or feature. Replace every `{{...}}`.

```
## US-{{NN}}: {{Title}}

- Figma: desktop `{{node-id}}`, mobile `{{node-id}}` (screenshots: `docs/figma/{{file}}.png`)
- Story: As a {{user}} I want {{goal}} so that {{benefit}}.

Acceptance criteria:
1. Given {{context}}, when {{action}}, then {{visible result}}.
2. ...

States:
- Designed: {{e.g. default, hover, filled}}
- Missing from design: {{e.g. loading, empty, error}} (see decisions #{{n}})

Out of scope: {{what this story does not cover}}
Assumptions: {{ASSUMPTION: ...}} or none
Open questions: decisions #{{n}} or none
```

Rules:
- Acceptance criteria describe what the user sees or does, numbered, each testable.
- Everything not visible in Figma is marked ASSUMPTION or raised as an open question.
