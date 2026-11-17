---
id: TASK-AIWS-006
title: Design CI CD quality gates and human approvals
feature: delivery-governance
status: IN_REVIEW
priority: HIGH
assignee: DEVOPS_ENGINEER
sprint: SPRINT-AIWS-001
estimate: 5
depends_on: [TASK-AIWS-002, TASK-AIWS-003, TASK-AIWS-005]
labels: [ci, cd, approvals]
---

## Objective
Define and harden the repository's current CI workflow, and document a safe path toward application CI/CD with explicit human approval before sensitive or production actions.

## Acceptance criteria
- [x] Current GitHub Actions workflows and their trigger/permission scopes are inventoried.
- [x] Required lint, typecheck, unit, integration, security, and build checks are mapped to project realities; non-existent application checks are explicitly conditional.
- [x] PR validation and branch protection expectations are documented, distinguishing recommendations from configured repository settings.
- [x] Deployment environments and approval gates are specified.
- [x] Database migration, secrets, rollback, and backup safety are covered.
- [x] No production deployment or destructive database operation can be treated as agent-authorized without explicit owner approval.
- [x] Existing dashboard workflow is hardened with least-privilege job permissions and safe deployment concurrency.
- [ ] DevOps review and Product Owner approval of the policy are pending.

## Implementation notes
- Policy document: [CI/CD and human approval policy](../../delivery/CI_CD_APPROVAL_POLICY.md).
- Current workflow: `.github/workflows/dashboard.yml`; it validates task metadata, builds the dashboard, and publishes GitHub Pages on main push or manual dispatch. It is not a full application pipeline.
- The current package scripts are `validate`, `dashboard:build`, and `dashboard:serve`; no application lint/typecheck/test/build scripts are defined.
- Workflow hardening scopes Pages write and OIDC token permissions to the deploy job; validation remains read-only.
- Branch protection, required reviewers, and GitHub Pages environment protections are repository settings and have not been changed or verified by this documentation task.
- No application database, migration runner, deployment target, or production environment is configured in this repository.

## Handoff
- **Task ID:** TASK-AIWS-006
- **Objective:** Establish least-privilege CI workflow behavior and an auditable approval model for current dashboard publication and future application delivery.
- **Context and constraints:** Markdown-first workspace template; only dashboard validation/build/Pages publishing exists. No app CI pipeline, application database, or production app environment is defined.
- **Changed files:**
  - `.github/workflows/dashboard.yml`
  - `docs/delivery/CI_CD_APPROVAL_POLICY.md`
  - `docs/tracking/tasks/TASK-AIWS-006-cicd-approval.md`
- **Decisions and references:** Workflow-level `contents: read`; job-level permissions; Pages write/OIDC permissions only in deploy job; deploy job concurrency does not cancel an in-progress publication. Policy distinguishes existing controls from proposed branch/environment protections. See [MCP tool security policy](../../security/MCP_TOOL_SECURITY_POLICY.md).
- **Validation evidence:** Reviewed `PROJECT_CONTEXT.md`, `WORKFLOW.md`, TASK-AIWS-006, `.github/workflows/dashboard.yml`, and `package.json` on main. Confirmed only three npm scripts exist and mapped quality gates accordingly. YAML syntax, `npm run validate`, `npm run dashboard:build`, and GitHub Actions execution are pending PR checks.
- **Risks and blockers:** Repository branch protection, required status checks, Pages environment reviewers, CODEOWNERS, application stack, deployment provider, database/backup strategy, and production approvers are not configured or verified by this task.
- **Next owner:** DevOps Engineer, then Product Owner.
- **Next step:** Review least-privilege workflow diff and approval matrix; inspect GitHub branch/environment settings; request changes or approve. Application-specific CI/CD should be a separately scoped task after the target application and deployment platform are selected.

## Review notes
- DevOps review: pending.
- Product Owner approval: pending.
