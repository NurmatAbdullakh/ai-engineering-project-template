---
id: TASK-AIWS-004
title: Define Figma to frontend implementation workflow
feature: design-to-code
status: IN_REVIEW
priority: HIGH
assignee: UI_UX_DESIGNER
sprint: SPRINT-AIWS-001
estimate: 3
depends_on: [TASK-AIWS-002]
labels: [figma, frontend, workflow]
---

## Objective
Define a repeatable design handoff from Figma to frontend implementation and visual QA using the connected Figma workflow and the repository's task process.

## Acceptance criteria
- [x] Figma file, page, frame, and prototype references are captured in task handoffs.
- [x] Responsive breakpoints, component states, interactions, accessibility, and design tokens are included where applicable.
- [x] Frontend implementation checklist and ambiguity escalation are documented.
- [x] Browser screenshot and visual comparison expectations are documented.
- [x] Figma MCP capabilities and limitations are verified before promising automated file access or edits.

## Implementation notes
The workflow is stack-aware but implementation-neutral. Storybook and Playwright are recommendations for a future application repository, not claimed dependencies of this Markdown-first workspace. No app frontend or automatic design-to-code pipeline was introduced.

## Handoff
- Owner: Product Owner for workflow review; UI/UX Designer and Frontend Engineer for follow-up adoption.
- Evidence:
  - `docs/workflows/FIGMA_TO_FRONTEND.md` — end-to-end process, tool capabilities/limitations, design preparation, implementation, visual QA, acceptance, roles, and security.
  - `docs/workflows/templates/FIGMA_HANDOFF_TEMPLATE.md` — copy-ready per-feature handoff checklist.
- Figma verification: Figma `whoami` returned an authenticated response. Available tool surface was inspected for metadata, design context, screenshots, Figma editing, libraries/variables, comments, Code Connect, web-page capture, and code import. No specific design file was inspected because no file URL/node was supplied.
- Limitations recorded: Figma design context is reference material to adapt; code import creates a Figma Code layer and does not guarantee native Design generation, application preview, or publishing. File access and edits depend on file key, node ID, and permissions.
- Validation: Repository-required task frontmatter fields and allowed status/priority values are used. GitHub Actions task validation/dashboard build are pending PR checks. No application code or runtime dependencies were added.
- Blockers: None for documenting the workflow. Applying it to a real screen requires the relevant Figma file and frame link.
- Next step: Product Owner reviews and approves the workflow; adopt the handoff template on the next frontend task.
