---
id: TASK-AIWS-002
title: Strengthen team workflow and approval rules
feature: workspace-foundation
status: IN_REVIEW
priority: CRITICAL
assignee: TEAM_LEAD
sprint: SPRINT-AIWS-001
estimate: 3
depends_on: [TASK-AIWS-001]
labels: [documentation, governance]
---

## Objective
Make the existing team workflow operationally clear, including task lifecycle, source of truth, execution protocol, handoffs, design review, tool safety, and human approval gates.

## Acceptance criteria
- [x] Existing role responsibilities are preserved and clarified.
- [x] The difference between role profiles and implemented orchestration is explicit.
- [x] Required task metadata and allowed statuses are documented.
- [x] A step-by-step execution protocol is documented.
- [x] Handoff requirements include evidence, blockers, and next owner.
- [x] Figma-to-frontend design handoff requirements are documented.
- [x] Least-privilege tool use and human approval gates are documented.

## Implementation notes
Updated `docs/team/WORKFLOW.md`. No orchestration runtime or automation behavior is claimed as implemented.

## Handoff
- Owner: Product Owner for review
- Evidence: Updated workflow on branch `feat/ai-workspace-foundation-sprint`
- Blockers: Approval of the proposed governance rules.
- Next step: Review and approve the workflow contract.
