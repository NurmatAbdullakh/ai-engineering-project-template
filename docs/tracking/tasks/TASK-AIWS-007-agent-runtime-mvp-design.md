---
id: TASK-AIWS-007
title: Design vendor-neutral agent runtime MVP
feature: agent-runtime-mvp
status: IN_REVIEW
priority: CRITICAL
assignee: SOFTWARE_ARCHITECT
sprint: SPRINT-AIWS-002
estimate: 8
depends_on: [TASK-AIWS-003, TASK-AIWS-005, TASK-AIWS-006]
labels: [architecture, agents, runtime, security]
---

## Objective
Translate the accepted coordinator-first concept into an implementable, vendor-neutral Agent Runtime MVP design with explicit execution, persistence, verification, security, and approval boundaries.

## Acceptance criteria
- [x] Runtime components and responsibilities are documented.
- [x] Model-provider abstraction and capability differences are addressed.
- [x] Agent invocation/result contracts and workflow states are specified.
- [x] Tool policy, approval, secret isolation, and verification boundaries are specified.
- [x] Persistence, audit, artifacts, budgets, retries, and recovery requirements are documented.
- [x] MVP scope, non-goals, delivery phases, and measurable acceptance criteria are defined.
- [x] Technology and deployment alternatives are recorded in ADR-0002.
- [ ] Product Owner approves ADR-0002 and open technology decisions.
- [ ] Security Reviewer approves the threat model and sandbox/tool boundary.

## Implementation notes
- Design document: [Agent Runtime MVP Technical Design](../../architecture/AGENT_RUNTIME_MVP_DESIGN.md).
- ADR: [ADR-0002](../../architecture/adr/ADR-0002-agent-runtime-mvp.md).
- The recommended baseline is Node.js + TypeScript modular monolith, local CLI, sequential execution, provider-neutral adapters, and a local transactional store candidate. These are proposals, not accepted decisions.
- Candidate model vendors include Anthropic, OpenAI, and Google; none is pre-approved by this design.
- No runtime code, credentials, external tools, or production resources were changed.

## Handoff
- **Task ID:** TASK-AIWS-007
- **Objective:** Obtain owner/security decisions required to implement a safe first runtime slice.
- **Changed files:**
  - docs/architecture/AGENT_RUNTIME_MVP_DESIGN.md
  - docs/architecture/adr/ADR-0002-agent-runtime-mvp.md
  - docs/tracking/tasks/TASK-AIWS-007-agent-runtime-mvp-design.md
- **Evidence:** The design maps to ADR-0001 coordinator-first workflow and the existing MCP/CI approval policies. It distinguishes local CLI MVP from a future hosted multi-user service.
- **Validation:** Documentation and task frontmatter reviewed for consistency with current project context and workflow. Runtime tests do not apply because no executable runtime was added.
- **Risks/blockers:** ADR-0001 is still marked proposed; Product Owner acceptance is required. Provider/data terms, initial provider, persistence choice, sandbox technology, and local execution boundary need explicit approval. Security review is required before enabling write-capable tools.
- **Next owner:** Product Owner, then Security Reviewer and Software Architect.
- **Next step:** Review ADR-0002 approval checklist. After decisions are accepted, split implementation into small tasks beginning with contracts, fake provider, and CLI skeleton.

## Review notes
- Product Owner review: pending.
- Security Reviewer review: pending.
