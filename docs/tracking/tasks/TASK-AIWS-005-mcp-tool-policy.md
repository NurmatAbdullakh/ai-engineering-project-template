---
id: TASK-AIWS-005
title: Define MCP tool access and safety policy
feature: tool-governance
status: TODO
priority: HIGH
assignee: SECURITY_REVIEWER
sprint: SPRINT-AIWS-001
estimate: 3
depends_on: [TASK-AIWS-002, TASK-AIWS-003]
labels: [mcp, security, governance]
---

## Objective
Define how agents discover and use connected MCP tools with least privilege, clear trust boundaries, and approval requirements for consequential actions.

## Acceptance criteria
- [ ] Tool inventory records purpose, owner, access scope, and supported actions.
- [ ] Read, write, destructive, and external-side-effect operations are classified.
- [ ] Per-agent allowlists and least-privilege rules are defined.
- [ ] Secret handling, prompt-injection resistance, and untrusted tool-output rules are documented.
- [ ] Approval requirements, audit logging, rate limits, and failure handling are specified.
- [ ] Policy is reviewed by Security Reviewer and approved by Product Owner.

## Implementation notes
Start with GitHub, Figma, Postman, Context7, and tldraw. Do not assume every connector exposes the same capabilities or that tool access can be technically restricted until verified.

## Handoff
- Owner: Security Reviewer
- Evidence:
- Blockers: Awaiting orchestration boundaries and tool inventory.
- Next step: Produce the MCP tool policy.
