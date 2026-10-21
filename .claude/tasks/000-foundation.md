# Task 000: Foundation

- Status: todo
- Gate: human
- Depends on: none
- Size: M

## Goal
A running empty app with the locked stack, design tokens extracted from Figma, base layout and a working test harness.

## References
- Spec: `docs/spec/` (stack and global requirements)
- Figma: variables/styles page `{{node-id}}`, one representative screen `{{node-id}}`
- Decisions: `docs/decisions.md`

## Scope
- Scaffold the project with the stack from CLAUDE.md, in this existing folder. Never modify or delete `CLAUDE.md`, `.mcp.json`, `.claude/` or `docs/`. If the scaffolder refuses a non-empty folder, scaffold into a temp subfolder and move the generated files up.
- Replace the boilerplate root `README.md` completely with one describing the app: what it is, how to install, run and test.
- Keep the existing `.gitignore` (stack-agnostic base). Append a section with the entries for the chosen stack: dependencies, build output, test reports, caches. If the scaffolder generates its own `.gitignore`, merge its entries into the existing file, never replace it.
- Extract design tokens from Figma MCP (colors, typography, spacing, radii, shadows, breakpoints) into `{{src/styles/tokens}}` and wire them into the styling setup.
- Base layout shell (page container, header/footer slots if present in the design) using tokens only.
- Unit test runner and Playwright e2e configured, each with one passing smoke test.
- Fill the Commands and Structure sections in CLAUDE.md with the real values.

## Out of scope
- Any feature screen or component beyond the layout shell.
- Any data fetching or business logic.

## Acceptance criteria
1. Install, dev, build, lint, typecheck, unit and e2e commands from CLAUDE.md all succeed.
2. Every token value matches the Figma variable/style it came from. No hardcoded values in the layout shell.
3. The layout shell renders at both viewports without horizontal scroll.

## Test scenarios
| ID | Type | Scenario | Expected | Covers AC |
|----|------|----------|----------|-----------|
| T1 | unit | Smoke test | passes | 1 |
| T2 | e2e | Open home route | page loads, no console errors | 1, 3 |
| T3 | e2e | Viewport 375 wide | no horizontal scroll | 3 |

## Validation steps
1. Run every command in CLAUDE.md Commands section.
2. Token report: table of token name, value in code, Figma source name and value.
3. Playwright MCP screenshots of the shell at both viewports.

## Human checkpoint
- Stack and folder structure acceptable.
- Token table matches Figma.
- Shell screenshots look right.

## Log

## Metrics
- Started:
- Finished:
- Human interventions: 0
- Rework loops: 0
- Notes on prompts that failed:
