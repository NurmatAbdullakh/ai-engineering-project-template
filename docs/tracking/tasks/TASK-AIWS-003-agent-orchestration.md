---
id: TASK-AIWS-003
title: Design multi-agent orchestration architecture
feature: agent-orchestration
status: IN_REVIEW
priority: HIGH
assignee: SOFTWARE_ARCHITECT
sprint: SPRINT-AIWS-001
estimate: 5
depends_on: [TASK-AIWS-001, TASK-AIWS-002]
labels: [architecture, agents]
---

## Objective
Design a safe, observable orchestration architecture that coordinates the documented specialist roles without assuming that role profiles already provide runtime agent collaboration.

## Acceptance criteria
- [x] Orchestration model, coordinator responsibilities, and agent invocation lifecycle are documented.
- [x] Task state, context passing, handoff payload, and artifact storage are defined.
- [x] Retry, timeout, cancellation, idempotency, and failure recovery are covered.
- [x] Human approval checkpoints and prohibited autonomous actions are explicit.
- [x] Observability, audit trail, and model/tool cost tracking are specified.
- [x] Alternatives and trade-offs are recorded in an ADR for owner review.

## Implementation notes
The proposal recommends a minimal coordinator-first logical architecture with sequential bounded specialist invocations. It deliberately does not select a runtime framework, database, queue, hosting platform, or model vendor. Runtime implementation is out of scope until the Product Owner accepts the ADR and follow-up technology decisions are made.

## Handoff
- Owner: Product Owner for architecture review; Software Architect for revisions.
- Evidence:
  - `docs/architecture/AGENT_ORCHESTRATION.md` — logical architecture, lifecycle, contracts, state, recovery, approval gates, tool/model boundaries, observability, cost controls, and implementation stages.
  - `docs/architecture/adr/ADR-0001-coordinator-first-orchestration.md` — alternatives, trade-offs, recommendation, and explicit approval checklist.
- Validation: Task metadata structure follows the repository's required frontmatter fields. GitHub Actions task validation and dashboard build are pending PR checks; no runtime tests apply because no runtime code was introduced.
- Blockers: Product Owner approval of ADR-0001 is required before runtime implementation.
- Next step: Product Owner reviews the proposal and either accepts the logical architecture or requests changes. After acceptance, select runtime technology in a separate decision.
