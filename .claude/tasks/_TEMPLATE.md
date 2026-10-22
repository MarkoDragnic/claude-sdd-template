# Task {{NNN}}: {{Short title}}

- Status: todo
- Gate: auto | human
- Depends on: {{000}}
- Size: S | M | L

## Goal
{{One sentence. What exists after this task that did not exist before.}}

## References
- Spec: `docs/spec/{{file}}` sections {{x.y}} or stories US-{{NN}}
- Figma: {{frame name}} desktop node `{{node-id}}`, mobile node `{{node-id}}`
- Decisions: `docs/decisions.md` #{{n}}

## Scope
- {{Concrete deliverable}}

## Out of scope
- {{What must NOT be built here, usually the next task's work}}

## Acceptance criteria
Numbered and testable. Tests reference these numbers.
1. {{Given / when / then}}

## Test scenarios
| ID | Type | Scenario | Expected | Covers AC |
|----|------|----------|----------|-----------|
| T1 | unit | {{happy path}} | {{...}} | 1 |
| T2 | e2e | {{edge case}} | {{...}} | 1 |
| T3 | e2e | {{error state}} | {{...}} | 2 |

## Validation steps (in this order, stop on first failure)
1. Lint + typecheck pass.
2. Unit tests pass.
3. E2E tests pass.
4. Visual: Figma MCP screenshot of each node vs Playwright MCP screenshot at the same viewport. List every difference.
5. Manual browser checks with Playwright MCP: {{interactions to click through}}.

## Human checkpoint (only if Gate: human)
- {{What exactly the human confirms}}

## Log
<!-- Claude appends: date, what was done, decisions, issues, validation results -->

## Metrics
- Started:
- Finished:
- Human interventions: 0
- Rework loops: 0
- Notes on prompts that failed:
