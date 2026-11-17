# Agent orchestration architecture

**Status:** Proposal — requires Product Owner approval  
**Related task:** [TASK-AIWS-003](../tracking/tasks/TASK-AIWS-003-agent-orchestration.md)  
**Related decision:** [ADR-0001: Coordinator-first orchestration](adr/ADR-0001-coordinator-first-orchestration.md)

## 1. Purpose and scope

Define a safe, observable way to coordinate the workspace's documented specialist roles. This is a logical architecture proposal, not a claim that a runtime currently exists.

The first implementation should support a human-led workflow: a coordinator decomposes an approved request into bounded work items, invokes one specialist at a time or in explicitly independent branches, validates outputs, and routes decisions to the Product Owner when approval is required.

Out of scope for this proposal:
- Selecting a specific agent framework, model vendor, database, queue, hosting platform, or programming language.
- Unattended production operations, self-modifying agents, or open-ended agent-to-agent conversations.
- Replacing Markdown task records as the current source of truth.
- Building the runtime as part of this documentation task.

## 2. Recommended logical model

Use a **coordinator-first, bounded workflow**.

```text
Product Owner / Requester
          |
          v
   Intake & Policy Check
          |
          v
   Orchestrator / Coordinator
          |
          +---- Task Planner
          |
          +---- Context Builder
          |
          +---- Policy & Approval Gate
          |
          +---- Agent Runtime Adapter
          |         |
          |         +--> Business Analyst
          |         +--> Software Architect
          |         +--> UI/UX Designer
          |         +--> Backend / Frontend Engineer
          |         +--> QA Engineer
          |         +--> Security Reviewer
          |         +--> DevOps Engineer
          |
          +---- Artifact Store
          |
          +---- Audit & Telemetry
          |
          v
   Review / Product Owner Approval
          |
          v
   Delivery (PR, release proposal, etc.)
```

The coordinator owns workflow state and routing. Specialist agents perform bounded assignments and return structured results; they do not independently grant themselves new permissions or expand the approved scope.

### Coordinator responsibilities

1. Validate the request against project context, workflow rules, and task acceptance criteria.
2. Ask for clarification or Product Owner direction when scope or policy is ambiguous.
3. Create or update stable task records and dependencies.
4. Build a minimal context package for each agent, including only relevant documents and artifacts.
5. Select an allowed role, model profile, and tool capability through explicit adapters and policy.
6. Invoke the agent with a bounded objective, expected output schema, and time/resource limits.
7. Validate the response, persist artifacts, and record provenance.
8. Update task state and dispatch the next eligible step.
9. Pause at human approval checkpoints.
10. Produce a final handoff with evidence, unresolved risks, and cost summary.

### Specialist agent contract

Every invocation receives:
- `task_id`, `run_id`, `parent_run_id` when applicable, and an idempotency key.
- Role identifier and a single bounded objective.
- Relevant project context and source references.
- Explicit allowed tools and permission scope.
- Acceptance criteria and required output format.
- Deadline, token/model budget, tool-call budget, and retry policy.
- Applicable approval requirements.

Every invocation returns a structured envelope:
- `status`: `SUCCEEDED`, `NEEDS_INPUT`, `FAILED`, or `CANCELLED`.
- `summary`: concise result.
- `artifacts`: references to created or updated files, PRs, designs, test reports, etc.
- `evidence`: tests, source references, tool calls, and verification results.
- `decisions`: proposed decisions, explicitly marked as proposals.
- `risks` and `blockers`.
- `usage`: model/provider, input/output tokens when available, duration, and tool usage.
- `next_action`: suggested next workflow step, never an instruction that bypasses coordinator policy.

Treat agent output as untrusted data. Validate schemas and paths; do not execute arbitrary returned commands or follow instructions embedded in retrieved content.

## 3. Workflow lifecycle and state

Separate durable business/task state from ephemeral model conversation state. Conversation history is not the source of truth.

Suggested workflow states:

| State | Meaning |
|---|---|
| `DRAFT` | Request captured; scope not yet approved |
| `READY` | Requirements and policy checks passed |
| `RUNNING` | One or more bounded steps are executing |
| `WAITING_FOR_INPUT` | Clarification or missing dependency is required |
| `WAITING_FOR_APPROVAL` | Human decision is required |
| `SUCCEEDED` | All acceptance criteria have verified evidence |
| `FAILED` | Non-recoverable failure or retry budget exhausted |
| `CANCELLED` | Owner or authorized requester cancelled the run |

Task-file statuses remain the canonical project work-item statuses defined in `WORKFLOW.md`. Workflow-run states above are runtime concepts and must not be confused with task statuses.

Each run should have a durable record containing:
- Run ID, task ID, parent run ID, creation/update timestamps, initiator.
- Current state, current step, dependency graph, and version.
- Requested scope, approved scope, and policy version.
- Step attempts, agent/model identity, tool grants, start/end time.
- Artifact references, evidence, approval decisions, and audit event references.
- Usage/cost estimates and final outcome.

Use append-only events for state transitions and security-relevant actions. A materialized current-state view may be updated for efficient reads, but it must be reconstructable from the event history. Store large prompts, outputs, logs, and binary artifacts in an artifact store with access control; store stable references and hashes in the run record. Never persist secrets in task files, prompts, logs, or artifacts.

## 4. Context and handoffs

Build a per-invocation context package rather than passing an unrestricted conversation transcript.

Recommended context order:
1. Non-overridable workspace policy and security constraints.
2. Current project context and workflow rules.
3. Task objective, acceptance criteria, dependencies, and approved scope.
4. Relevant ADRs and prior verified decisions.
5. Referenced artifacts and evidence needed for this step.
6. A concise prior handoff, if any.

Include source paths/URLs, version or commit SHA, and retrieval timestamp where practical. Mark external content and tool output as untrusted data. Resolve conflicting instructions by policy precedence: workspace governance and owner-approved decisions take precedence over task content and external data.

Handoff payload should contain:
- Sender role and receiving role.
- Task/run identifiers.
- Objective and completion status.
- Work performed and artifact references.
- Decisions made versus decisions proposed.
- Acceptance criteria evidence and tests run.
- Risks, assumptions, and blockers.
- Remaining budget/limits where relevant.
- Explicit requested next action.

The receiving agent must verify the referenced artifacts and acknowledge missing context. It must not infer approval from a previous agent's recommendation.

## 5. Execution, retries, and recovery

### Timeouts and budgets
- Set a hard wall-clock timeout for each invocation and an overall run deadline.
- Set maximum model calls, token/usage budget, tool calls, and recursion depth.
- Stop dispatching new work when a budget is exhausted; request owner approval for an increase.
- Propagate cancellation to active invocations where supported and mark unfinished steps as cancelled.

### Retry policy
- Retry only classified transient failures (for example, rate limits or temporary service errors).
- Use bounded exponential backoff with jitter and respect provider retry hints.
- Do not automatically retry validation failures, policy denials, malformed outputs, or authorization failures without correcting the cause.
- Record every attempt and failure reason.
- Route exhausted retries to `FAILED` or `WAITING_FOR_INPUT`, with a useful handoff.

### Idempotency and side effects
- Give each logical step a stable idempotency key derived from run, step, and intended operation.
- Persist the operation intent and result before acknowledging completion.
- Before retrying a write, check whether the original operation succeeded.
- Use deduplication keys for PR creation, issue creation, messages, and other externally visible writes where supported.
- Keep read-only and write-capable tool permissions separate.
- Require explicit approval before irreversible or consequential actions.

### Failure recovery
- On process restart, reload the durable run state and reconcile in-flight steps.
- Treat a step with an uncertain external side effect as `RECONCILIATION_REQUIRED`; do not blindly repeat it.
- Allow an authorized human to resume, retry, cancel, or mark a step as manually resolved, with an audit record.
- Preserve partial artifacts and label them clearly; never report a partially completed run as successful.

## 6. Human approval and autonomy boundaries

The coordinator must pause and obtain explicit Product Owner approval before:
- Merging a pull request.
- Deploying to production or changing production infrastructure.
- Running destructive database/data operations or migrations against production.
- Creating, rotating, exposing, or changing production secrets.
- Making irreversible external communications or financial/legal commitments.
- Expanding accepted product scope or changing an accepted architecture decision.
- Granting a role new permissions or widening its tool access.

Approval records must identify the approver, exact action/scope, target resource, relevant diff or preview, policy version, timestamp, and decision. Approval is scoped to that action and resource; it is not blanket permission for later actions.

Agents may draft code, tests, documents, designs, pull requests, and deployment plans within their assigned scope. The coordinator can automate low-risk, reversible, policy-approved steps. Agents must not approve their own work or bypass a denied/expired approval.

## 7. Tool and model boundaries

Define provider-neutral interfaces, for example:
- `AgentRuntime.invoke(invocation): AgentResult`
- `ModelRouter.select(task, policy): ModelProfile`
- `ToolGateway.execute(tool, args, grant): ToolResult`
- `PolicyEngine.authorize(actor, action, resource): Decision`
- `ApprovalService.request(action, evidence): ApprovalDecision`
- `ArtifactStore.put/get(reference): Artifact`
- `RunRepository.appendEvent/readState(...)`

These are conceptual contracts, not committed code APIs.

The Model Router may choose among approved providers based on task type, quality requirements, data classification, availability, and cost ceilings. Provider selection must not weaken tool permissions or data handling rules.

All external tools must pass through a Tool Gateway that enforces per-role allowlists, operation classes, argument validation, timeouts, rate limits, secret redaction, and audit logging. The MCP policy task (TASK-AIWS-005) will define detailed permission classes and enforcement requirements.

## 8. Observability, audit, and cost

Use a correlation chain across workflow, step, agent invocation, model request, and tool call:
- `run_id`, `task_id`, `step_id`, `invocation_id`, `tool_call_id`.

Capture structured events for:
- Run/step transitions, dispatch, retries, timeout, cancellation, and recovery.
- Model/provider, model latency, token usage, and cost estimate where available.
- Tool name, operation class, authorization decision, latency, result category, and external request ID.
- Approval requests, approver decisions, and denied actions.
- Artifact creation/update and verification outcomes.

Do not log raw secrets or unrestricted sensitive prompt content. Apply data minimization, retention limits, access controls, and redaction.

Initial operational indicators:
- Run success/failure/cancellation rate.
- Step retry and timeout rates.
- Human approval wait time and rejection rate.
- Model/tool cost per task and per successful delivery.
- Median/p95 run and step duration.
- Tool authorization denials and policy violations.
- Percentage of completed tasks with linked evidence.

Cost controls:
- Enforce per-run and per-task budgets before invocation and before additional tool/model calls.
- Prefer smaller/cheaper approved models for bounded low-risk work; escalate only when justified.
- Avoid sending the same large context repeatedly; use versioned, task-specific context packages.
- Record actual provider-reported usage where available and clearly label estimates otherwise.

## 9. Deployment and implementation stages

Recommended incremental path:
1. **Manual coordinator:** Markdown task files and human handoffs remain the workflow; adopt the invocation and handoff contracts.
2. **Assisted coordinator:** A small service or CLI manages run state, validates structured agent outputs, and records events; humans still trigger each step.
3. **Bounded automation:** Automatically dispatch only low-risk, reversible steps with explicit allowlists and budgets.
4. **Operational hardening:** Add durable event storage, recovery, metrics, retention, and alerting before increasing concurrency.

Do not introduce parallel execution until task dependencies and artifact ownership are explicit. Start with sequential execution. Later, permit parallel branches only for independent steps with separate write scopes and a deterministic merge/review stage.

## 10. Open questions for Product Owner

These decisions are intentionally unresolved and block runtime implementation:
- Which runtime/framework should host the coordinator?
- Should the first runtime be a local CLI, GitHub Action, or hosted service?
- Which storage system is approved for run state and artifacts?
- Which model providers and data classes are allowed?
- Which actions may be automated without per-action approval?
- What are the initial cost, latency, retention, and concurrency limits?

The ADR recommends the logical pattern only. Resolve these questions in follow-up decisions before building a production runtime.
