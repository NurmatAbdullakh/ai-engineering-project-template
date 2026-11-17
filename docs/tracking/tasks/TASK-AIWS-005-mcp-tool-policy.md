---
id: TASK-AIWS-005
title: Define MCP tool access and safety policy
feature: tool-governance
status: IN_REVIEW
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
- [x] Tool inventory records purpose, owner, access scope, and supported actions at a policy level; unverified connector capabilities are explicitly marked.
- [x] Read, reversible write, consequential write, and destructive/privileged operations are classified.
- [x] Per-agent role capability matrix and least-privilege rules are defined for all 11 workspace roles.
- [x] Secret handling, prompt-injection resistance, and untrusted tool-output rules are documented.
- [x] Approval requirements, audit logging, rate limits, failure handling, grant lifecycle, and incident response are specified.
- [ ] Policy is reviewed by Security Reviewer and approved by Product Owner.

## Implementation notes
- Policy document: [MCP tool security and permission policy](../../security/MCP_TOOL_SECURITY_POLICY.md).
- Start with GitHub, Figma, Postman, Context7, and tldraw as documented integrations; this is not a verified exhaustive inventory of every connector operation.
- The policy is a governance baseline, not an implemented enforcement runtime. Per-role tool allowlists, credential isolation, and centralized policy enforcement must be implemented and tested before autonomous write-capable execution.
- No connector trust level or permission is inferred solely from successful authentication or tool availability.

## Handoff
- **Task ID:** TASK-AIWS-005
- **Objective:** Establish least-privilege, auditable, human-governed MCP/tool access rules for the 11 documented roles.
- **Context and constraints:** Markdown-first workspace; coordinator-first orchestration remains a proposal; no centralized Tool Gateway or automatic approval runtime is implemented. Existing integrations are GitHub, Figma, Postman, Context7, and tldraw.
- **Changed files:**
  - docs/security/MCP_TOOL_SECURITY_POLICY.md
  - docs/tracking/tasks/TASK-AIWS-005-mcp-tool-policy.md
- **Decisions and references:** Proposed R0/W1/W2/D3 operation risk model (read, reversible write, consequential write, destructive/privileged); T0–T3 connector trust levels; default-deny and per-task grants; approval for consequential actions. See [ADR-0001](../../architecture/adr/ADR-0001-coordinator-first-orchestration.md) and [orchestration proposal](../../architecture/AGENT_ORCHESTRATION.md). These are policy proposals pending owner/security approval.
- **Validation evidence:** Reviewed current project context, workflow, task, orchestration proposal, and ADR on main; checked that all 11 documented roles are represented; checked that connector capabilities are not overstated. Repository CI validation and dashboard build are pending PR checks.
- **Risks and blockers:** Central enforcement, per-agent credentials, secret manager, audit storage, numeric quotas, and formal connector trust review do not yet exist or have not been selected. W2/D3 and write-capable autonomous execution must remain human-supervised until enforcement is implemented and verified.
- **Next owner:** Security Reviewer, then Product Owner.
- **Next step:** Review the risk classes, role matrix, connector trust assumptions, and approval rules; request changes or approve the policy. Runtime implementation should be a separately scoped task/ADR.

## Review notes
- Security Reviewer review: pending.
- Product Owner approval: pending.
