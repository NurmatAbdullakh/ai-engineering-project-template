# ADR-0001: Coordinator-first agent orchestration

- **Status:** Proposed — Product Owner approval required
- **Date:** 2026-11-17
- **Decision owners:** Product Owner, Software Architect
- **Related task:** TASK-AIWS-003
- **Related proposal:** [Agent orchestration architecture](../AGENT_ORCHESTRATION.md)

## Context

The workspace documents 11 specialist roles, but currently has no agent orchestration runtime, durable run-state service, shared memory implementation, or centralized tool-permission enforcement. Role descriptions alone do not create reliable collaboration.

The workspace needs a path from human-led, auditable handoffs toward bounded automation without granting agents open-ended autonomy. The repository is currently Markdown-first, and the concrete application stack, hosting target, database, model providers, and runtime framework have not been selected.

## Decision

**Propose a coordinator-first, bounded workflow architecture.**

A single logical orchestrator owns task decomposition, dependency scheduling, context assembly, role invocation, state transitions, output validation, policy checks, approval gates, artifact references, and final handoffs.

Specialist agents are invoked as bounded workers. Each receives a scoped objective, minimum relevant context, explicit tool grants, acceptance criteria, and execution budgets. Each returns a structured result with status, artifacts, evidence, risks, blockers, and usage.

The initial workflow should be sequential. Parallel execution may be added only for demonstrably independent steps with isolated write scopes and a deterministic integration/review stage.

Durable workflow state and append-only audit events are separate from ephemeral model conversation history. Large artifacts live in an access-controlled artifact store; task records and run records retain stable references and provenance.

Model providers and MCP/external tools sit behind provider-neutral interfaces and a policy-enforcing gateway. Human approval is mandatory for merges, production deployments, destructive data operations, production-secret changes, irreversible external actions, scope changes, and accepted architecture changes.

This is a proposed logical architecture only. It does not select a runtime framework, programming language, database, queue, hosting provider, or model vendor.

## Alternatives considered

### A. Direct agent-to-agent collaboration / open-ended swarm

Agents can message and delegate to each other with few central constraints.

**Pros**
- Flexible collaboration and potentially useful parallel exploration.
- Less centralized workflow code for some interactions.

**Cons**
- Harder to bound scope, cost, recursion, and tool permissions.
- Difficult to reconstruct why an action happened or which agent authorized it.
- More complex cancellation, retries, idempotency, and failure recovery.
- Human approval and audit boundaries become harder to enforce consistently.

**Assessment:** Rejected for the initial system. Reconsider only for narrowly scoped, measurable use cases after the coordinator and policy layer are proven.

### B. Single general-purpose agent

One agent performs planning, design, coding, testing, and review.

**Pros**
- Simple runtime and fewer handoffs.
- Lower orchestration overhead.

**Cons**
- Weak separation of duties and review independence.
- Context grows quickly and mixes unrelated concerns.
- Harder to apply role-specific tool grants and specialist acceptance criteria.

**Assessment:** Not selected as the default. A single agent may still be used for small bounded tasks.

### C. Coordinator-first workflow with specialist workers

A central coordinator manages explicit workflow state and invokes role-specific workers.

**Pros**
- Centralized policy enforcement, audit, budgets, approvals, and recovery.
- Clear task ownership and structured handoffs.
- Supports gradual evolution from manual to assisted to bounded automation.
- Allows model/runtime providers to change behind interfaces.

**Cons**
- Coordinator becomes a critical component and must be made reliable.
- More explicit contracts and state management are required.
- Sequential execution can increase latency until safe parallelism is introduced.

**Assessment:** Recommended. It best fits the current human-led governance model and incremental maturity of this repository.

## Consequences

### Positive
- Clear boundary between orchestration policy and specialist reasoning.
- Easier to audit and explain task outcomes.
- Explicit handling of retries, timeouts, cancellation, idempotency, and human approvals.
- Avoids premature commitment to a framework or infrastructure.
- Enables incremental implementation without pretending the runtime already exists.

### Negative / trade-offs
- Requires durable state, event history, schema validation, and artifact lifecycle design.
- Coordinator availability and correctness become important.
- Additional structured handoffs and validation add latency and implementation effort.
- Cost and observability instrumentation must be built rather than inferred from chat history.

## Security and governance

- Agent output and retrieved content are untrusted data.
- Tools are invoked only through explicit, role-scoped grants.
- Read-only and write/destructive actions are distinct permission classes.
- No agent may approve its own action or widen its own permissions.
- Approval is explicit, scoped to an exact action/resource, time-stamped, and auditable.
- Secrets and sensitive content must be excluded from task files and redacted from logs.
- Human approval remains required for all actions listed in the project workflow and architecture proposal.

## Implementation implications

1. Adopt the logical invocation, result, context, and handoff contracts from the proposal.
2. Keep Markdown task files as the current project work-item source of truth.
3. Select the runtime, persistence, hosting, provider, and model technologies in separate decisions after requirements and constraints are known.
4. Implement policy enforcement and audit before enabling autonomous write-capable tool use.
5. Start sequentially; add parallel branches only with explicit dependency and write-scope controls.
6. Validate recovery and idempotency before enabling unattended retries of external side effects.

## Approval

- [ ] Product Owner accepts the coordinator-first logical architecture.
- [ ] Product Owner confirms that runtime technology selection remains a separate decision.
- [ ] Security Reviewer reviews the proposed permission and approval boundaries before any write-capable runtime is implemented.

Until approval is recorded, this ADR remains a proposal and must not be treated as an accepted architecture decision.
