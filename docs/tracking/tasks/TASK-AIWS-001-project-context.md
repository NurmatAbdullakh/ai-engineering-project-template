---
id: TASK-AIWS-001
title: Define AI Engineering Workspace project context
feature: workspace-foundation
status: IN_REVIEW
priority: CRITICAL
assignee: TEAM_LEAD
sprint: SPRINT-AIWS-001
estimate: 2
depends_on: []
labels: [documentation, foundation]
---

## Objective
Replace the starter project context with an accurate description of the AI Engineering Workspace, its boundaries, existing capabilities, constraints, and current implementation state.

## Acceptance criteria
- [x] Product purpose, target users, value, and repository scope are documented.
- [x] Existing tools and automation are distinguished from planned capabilities.
- [x] Agent role profiles are not misrepresented as a working orchestration runtime.
- [x] Security, human approval, traceability, and generated-file constraints are documented.
- [x] Current milestone, sprint, known blockers, and unimplemented capabilities are explicit.

## Implementation notes
Updated `docs/team/PROJECT_CONTEXT.md`. The workspace repository is treated as a Markdown-first engineering operating system, not as the POS SaaS application.

## Handoff
- Owner: Product Owner for review
- Evidence: Updated project context on branch `feat/ai-workspace-foundation-sprint`
- Blockers: Confirm any missing product or infrastructure details during review.
- Next step: Review and approve the project context.
