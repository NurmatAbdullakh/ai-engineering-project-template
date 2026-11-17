# CI/CD and human approval policy

**Status:** Proposed operating policy; Product Owner and DevOps review required  
**Related task:** [TASK-AIWS-006](../tracking/tasks/TASK-AIWS-006-cicd-approval.md)  
**Related policy:** [MCP tool security and permission policy](../security/MCP_TOOL_SECURITY_POLICY.md)  
**Related architecture:** [Agent orchestration architecture](../architecture/AGENT_ORCHESTRATION.md)

## 1. Purpose and current boundary

This document defines delivery quality gates, repository protections, environment promotion, and human approval requirements for the AI Engineering Workspace.

The repository is a Markdown-first engineering template, not a deployed application. At the time of this review, the only checked-in GitHub Actions workflow is `.github/workflows/dashboard.yml`. It validates task metadata, builds the task dashboard, and publishes that dashboard to GitHub Pages. It is **not** a full application CI/CD pipeline.

No application frontend/backend, application test suite, database migration pipeline, production application environment, or application deployment target is currently defined in this repository. Recommendations for those capabilities below are conditional requirements for a future application, not claims about existing infrastructure.

## 2. Current workflow inventory

### `.github/workflows/dashboard.yml`

| Trigger | Current behavior | Delivery impact |
|---|---|---|
| Pull request with configured paths | Runs task validation and dashboard build; does not deploy Pages | PR quality gate for task/dashboard changes |
| Push to `main` with configured paths | Validates, builds, uploads Pages artifact, then deploys | Publishes the dashboard after a successful main-branch run |
| Manual `workflow_dispatch` | Runs validation/build, uploads artifact, then deploys | Allows a manually initiated dashboard publication |

Current commands:
- `npm run validate` — validates task frontmatter metadata.
- `npm run dashboard:build` — generates dashboard output.
- `npm run dashboard:serve` — local preview helper; not currently a CI gate.

The workflow uses Node.js 22, `actions/checkout@v4`, `actions/setup-node@v4`, `actions/upload-pages-artifact@v3`, and `actions/deploy-pages@v4`.

### Workflow hardening applied with this task

- Set workflow-level permissions to `contents: read` as the default.
- Kept the validation job read-only.
- Moved `pages: write` and `id-token: write` into the deploy job only.
- Made deploy depend on successful validation/build.
- Scoped deployment concurrency to the Pages deploy job and disabled cancellation of an in-progress publication.
- Kept PR runs non-deploying; only main pushes and manual dispatch publish Pages.

These are least-privilege improvements for the existing dashboard workflow. They do not configure repository branch protection or prove that a GitHub environment has required reviewers configured.

## 3. Quality gates by repository reality

### Required now

| Gate | When | Evidence / pass condition |
|---|---|---|
| Task metadata validation | PRs touching task/dashboard/script/package/workflow paths | `npm run validate` exits successfully |
| Dashboard generation | Same PRs | `npm run dashboard:build` exits successfully |
| Workflow review | Any change under `.github/workflows/` | Human review of triggers, permissions, secrets, and deployment effects |
| Documentation review | Policy, architecture, or workflow changes | Relevant owner/reviewer checks acceptance criteria and handoff evidence |

A PR is not ready to merge if a required check fails, is missing, or has not completed. A green dashboard build does not imply application code has been linted, typechecked, tested, or security-scanned.

### Add only when an application exists

| Gate | Applies when | Expected coverage |
|---|---|---|
| Formatting / lint | Source code and lint config exist | Deterministic formatter/linter; fail on violations |
| Typecheck | Typed application code exists | Compiler/type checker in CI |
| Unit tests | Testable application modules exist | Fast isolated tests; publish results |
| Integration tests | Database, queues, APIs, or external adapters exist | Ephemeral/test dependencies and isolated test credentials |
| Contract/API tests | API contracts or collections exist | Schema compatibility and non-production contract assertions |
| Build | Application build configuration exists | Reproducible production build |
| Dependency / secret / static security scan | Dependency manifests and source exist | Vulnerability, secret, and static-analysis checks with reviewed severity policy |
| E2E tests | A runnable app and stable test environment exist | Critical user journeys against a disposable or staging environment |

Do not add placeholder scripts or claim gates pass when their application targets do not exist. Introduce each gate with the application and record tool choice, scope, and exceptions.

## 4. Pull request and branch protection policy

Configure repository rulesets or branch protection for `main` (these settings are recommendations; this task does not claim to have changed GitHub repository settings):

1. Require pull requests; prohibit direct pushes and force pushes to `main`.
2. Require at least one approval from an authorized human reviewer. For security-sensitive changes, require Security Reviewer review in addition to owner approval where supported.
3. Require the applicable status checks, including task validation and dashboard build for their path scope.
4. Require branches to be up to date before merge when practical.
5. Dismiss stale approvals after new commits that materially change the reviewed diff.
6. Require conversation resolution and disallow bypass except for a documented repository-owner emergency procedure.
7. Prevent the PR author from satisfying an independent approval requirement for their own change.
8. Use CODEOWNERS for `docs/security/`, `docs/architecture/`, `.github/workflows/`, and other sensitive paths once the responsible GitHub teams/users are explicitly selected.

Do not invent reviewer usernames or mark checks as required until the exact GitHub check names have been observed on successful runs. Verify ruleset behavior with a test PR before relying on it.

## 5. Environment and promotion model

The current known deployment environment is GitHub Pages environment `github-pages`, used for the task dashboard. No application dev/staging/prod environments are configured by this repository.

For a future application, use distinct environments:

| Environment | Purpose | Credentials / data | Promotion |
|---|---|---|---|
| Development | Continuous integration and developer verification | Synthetic data; non-production credentials | Automated after PR checks or on approved development branch |
| Staging | Pre-production acceptance and integration validation | Isolated staging data and credentials | Explicit promotion after CI and QA evidence |
| Production | User-facing live service | Production secrets and real data | Explicit Product Owner approval and required technical/security review |

Environment protection requirements:
- Keep production secrets in the protected environment or approved secret manager, never in repository files or agent context.
- Require designated human reviewers for production environment deployments.
- Prevent self-review/self-approval where platform controls permit.
- Limit deployment credentials to the exact environment and operation.
- Use immutable build artifacts promoted between environments rather than rebuilding different source for production.
- Record commit SHA, artifact identity, workflow run, approver, deployment actor, and outcome.

The GitHub Pages environment should be inspected in repository settings to confirm whether required reviewers and branch restrictions are configured. Workflow YAML alone does not establish those settings.

## 6. Production deployment approval

Before any production deployment:
1. CI checks for the exact commit must pass.
2. QA must provide relevant test evidence and known-risk summary.
3. DevOps must confirm operational readiness, rollback procedure, monitoring, and target environment.
4. Security Reviewer must review security-sensitive changes or exceptions.
5. Product Owner must explicitly approve the exact commit/artifact and production target.
6. A human-authorized deployment job must execute only the approved artifact.
7. Verify health and critical user journeys; record deployment outcome.

An agent may prepare release notes, deployment plans, evidence, and a draft PR. It must not approve its own deployment, bypass environment protection, or trigger a production deployment without the explicit approval above.

Approval is scoped to a specific commit/artifact, environment, and deployment window. A changed artifact or material scope requires renewed approval.

## 7. Database migrations, backups, and rollback

No application database or migration pipeline is currently defined in this repository. When one is introduced:

- Run migration validation and integration tests against ephemeral or dedicated non-production databases first.
- Review migration SQL and expected locking, data volume, compatibility, and downtime risks.
- Prefer backward-compatible expand/contract migrations across application versions.
- Require explicit Product Owner approval and DBA/DevOps review for destructive production migrations, data deletion, irreversible transformations, and high-risk schema changes.
- Take and verify a recoverable backup or snapshot before high-risk production migrations; define restore owner, RPO/RTO, and restore verification.
- Never use production customer data in CI by default.
- Separate migration credentials from application runtime credentials and grant only required schema privileges.
- Make migration execution observable and idempotent where feasible; record migration identifier, commit, target, operator, approval, and result.
- Define rollback or forward-fix steps before execution. A database rollback is not assumed safe merely because application code can be reverted.
- Stop on unexpected row counts, lock duration, schema drift, or failed post-migration checks; escalate rather than improvising destructive repair.

Agents may draft migration plans and tests. They must not execute destructive production migrations or restore production data without explicit owner approval and authorized human execution.

## 8. Secrets and workflow security

- Use GitHub Actions secrets or protected environment secrets only for credentials that workflows need; use an approved external secret manager where required.
- Prefer OIDC/federated short-lived credentials over long-lived cloud keys when the target provider supports it.
- Never echo secrets, place them in artifacts, include them in task files, or expose them to model context.
- Grant each job only the GitHub token permissions it needs; use workflow-level read-only defaults and job-level elevation only where required.
- Pin third-party actions to reviewed full commit SHAs for hardened production workflows; review updates deliberately. Existing action tags are recorded in the current inventory and are not claimed to be immutable.
- Restrict workflow triggers and path scopes; carefully review `pull_request_target`, `workflow_run`, and untrusted fork execution before introducing them.
- Avoid executing untrusted PR code with privileged secrets or write tokens.
- Set bounded timeouts, concurrency groups, and cancellation behavior appropriate to each job.
- Use artifact retention and access settings appropriate to the data; do not upload secrets or sensitive test output.
- Review dependency updates and workflow permission changes as security-sensitive code changes.

## 9. Rollback and operational evidence

For application deployments, define:
- Artifact and source commit identity.
- Deployment health checks and success criteria.
- Rollback trigger thresholds and responsible human.
- Previous known-good artifact and rollback procedure.
- Database compatibility constraints and recovery plan.
- Monitoring/log references and incident escalation route.

A failed health check should halt further promotion. Rollback must be performed by an authorized human or an explicitly approved deployment mechanism; never let an agent improvise destructive cleanup. Preserve deployment and approval evidence.

For the current dashboard, recovery is a redeployment of a known-good main commit through the GitHub Pages workflow, subject to repository/environment permissions. Verify the actual Pages configuration and artifact history before relying on a recovery time.

## 10. CI/CD approval matrix

| Action | Agent may prepare | Required approval before execution |
|---|---|---|
| Run read-only PR validation | Yes | No additional approval beyond task scope |
| Build dashboard artifact in CI | Yes, through approved workflow | No, when checks are configured as documented |
| Merge PR | Yes, prepare PR and evidence | Product Owner / authorized human reviewer |
| Publish current dashboard from main | Workflow may run on main push after merge | Human PR approval and repository merge controls; verify Pages environment protections |
| Change workflow permissions or protected branch rules | Prepare proposed diff | Product Owner + Security Reviewer / repository administrator |
| Deploy application to staging | Prepare release and evidence | DevOps/QA approval per future environment policy |
| Deploy application to production | Prepare release and evidence only | Explicit Product Owner approval plus DevOps and required QA/Security review |
| Run destructive production migration / restore | Prepare plan and verification checklist only | Explicit Product Owner approval plus authorized DBA/DevOps execution |
| Create/rotate production secrets | Prepare change plan only | Product Owner + Security Reviewer + authorized secret owner |

No approval is inferred from a green check, merged PR, a prior release, or tool availability. The approval must be explicit and tied to the target action.

## 11. Workflow change validation

For every workflow change:
1. Inspect trigger conditions, event payload trust, job dependencies, and concurrency.
2. Check workflow-, job-, and environment-level permissions.
3. Confirm secrets are unavailable to untrusted code paths.
4. Validate YAML syntax and action inputs.
5. Run the repository's relevant npm validation/build scripts.
6. Open a PR and inspect the actual GitHub Actions run and check names.
7. Confirm the workflow does not deploy from PR events.
8. Review environment protections in GitHub settings separately from YAML.
9. Record test output, unrun checks, and limitations in the task handoff.

## 12. Open decisions

The following need owner/DevOps review before an application delivery pipeline is designed:
- Which application repository and stack will this template target?
- Which CI test, lint, security scanning, and artifact tools are selected?
- Which deployment provider and dev/staging/prod environments are required?
- Who are the authorized reviewers for production and security-sensitive changes?
- Which branch rulesets and required check names will be enforced?
- Which database, backup provider, retention, RPO/RTO, and migration strategy apply?

Record accepted choices in the relevant ADR or repository settings. Do not claim these controls exist until they are configured and verified.
