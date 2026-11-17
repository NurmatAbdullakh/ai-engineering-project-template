# Agent Runtime MVP — Technical Design

**Status:** Proposal — Product Owner and Security Reviewer approval required  
**Related architecture:** [Coordinator-first orchestration](AGENT_ORCHESTRATION.md)  
**Related ADR:** [ADR-0002: Agent runtime MVP technology and deployment](adr/ADR-0002-agent-runtime-mvp.md)  
**Related security policy:** [MCP tool security policy](../security/MCP_TOOL_SECURITY_POLICY.md)

## 1. Goal

Turn the documented coordinator-first architecture into a small, testable runtime that can execute a bounded engineering task through specialist agents, preserve run state, verify outputs, and stop at approval gates.

The MVP is a development tool, not an unattended production agent platform. It must be useful with one model provider first while keeping provider-specific code behind adapters.

### MVP success scenario

A human submits a task such as “review this feature branch and prepare a test plan”. The runtime:
1. validates the task and the caller's allowed scope;
2. builds a minimal, versioned context package;
3. asks the coordinator to produce a bounded execution plan;
4. invokes one specialist at a time;
5. validates each structured result;
6. runs only allowlisted, low-risk verification commands;
7. stores run/step state and audit events;
8. pauses for explicit approval before consequential writes;
9. produces a final report with artifacts, tests, risks, and estimated usage.

## 2. Non-goals for MVP

- Open-ended agent-to-agent chat or autonomous swarms.
- Parallel write-capable agents.
- Automatic PR merge, production deployment, production database operations, or secret changes.
- Browser/computer use or unrestricted shell access by agents.
- Automatic discovery of every installed MCP tool.
- Long-term semantic memory or cross-project user profiling.
- Training/fine-tuning models.
- Claiming provider feature parity; adapters expose only common capabilities.

## 3. Logical architecture

```text
CLI / Human Request
       |
       v
Request Validator ---- Project Context Loader
       |                         |
       v                         v
Coordinator / Planner <--- Versioned Context Package
       |
       v
Workflow Engine (sequential state machine)
       |
       +--> Model Router --> Provider Adapter --> LLM API
       |
       +--> Policy Engine --> Tool Gateway --> Explicit Tool Adapters
       |
       +--> Approval Gate --> Human decision
       |
       +--> Output / Schema Validator
       |
       +--> Verification Runner (allowlisted commands only)
       |
       +--> Run Repository + Append-only Audit Events
       |
       v
Final Handoff / Report
```

### Component responsibilities

| Component | Responsibility | Must not do |
|---|---|---|
| CLI/API boundary | Authenticate local operator, validate request, start/status/cancel runs | Bypass workflow policy |
| Coordinator | Decompose approved scope, order steps, select role/model profile | Execute tools directly or approve actions |
| Workflow engine | State transitions, dependencies, retries, deadlines, cancellation | Treat model conversation as durable state |
| Model router | Select an enabled provider/model under policy and budget | Change tool grants or data classification |
| Provider adapter | Normalize request, response, usage, timeout and provider errors | Expose provider-specific semantics to core |
| Policy engine | Default-deny decision for actor + operation + resource + environment | Grant permissions based on model output |
| Tool Gateway | Validate and execute a granted tool operation; redact and audit | Accept arbitrary shell/code from model |
| Approval service | Record exact action, target, preview, approver, expiry and decision | Treat vague “yes” as blanket approval |
| Run repository | Persist run/step state and append-only events | Store raw secrets |
| Artifact store | Store bounded artifacts with hash and access metadata | Make arbitrary local paths accessible |
| Verification runner | Execute named, configured checks in isolated workspace | Execute free-form model-generated commands |

## 4. Vendor-neutral model integration

Define a small common interface, for example:

- `ModelProvider.generate(request): Promise<ModelResponse>`
- `ModelProvider.supports(capability): boolean`
- `ModelResponse`: content blocks, structured output (when supported), usage, finish reason, provider request ID.
- `ModelProfile`: provider ID, model ID, allowed task classes, data classification, timeout, token and cost ceilings.

Initial provider adapters can include Anthropic, OpenAI, and Google Gemini, but enable only providers whose API credentials, data terms, and model IDs have been reviewed. Add one provider first; prove the contract with contract tests before adding another.

The core must not assume that all providers support identical tool calling, JSON schema enforcement, caching, prompt caching, streaming, or reasoning controls. Adapter capabilities must be explicit. If a capability is missing, the core uses a safe fallback or rejects the requested workflow.

Never silently switch provider if the fallback would change data handling, residency, cost ceiling, or approval assumptions. Provider fallback must be policy-approved and recorded.

## 5. Agent and workflow contracts

### Invocation

Each agent invocation includes:
- `runId`, `stepId`, `taskId`, `attempt`, idempotency key;
- role, bounded objective, approved scope and acceptance criteria;
- context references with commit/version/hash;
- allowed model profile and explicit tool grant IDs;
- output schema, deadline, token/cost/tool-call budgets;
- untrusted input labels and policy version.

### Result

Each agent returns a validated envelope:
- status: `SUCCEEDED | NEEDS_INPUT | FAILED | CANCELLED`;
- summary and structured output;
- artifact references and hashes;
- evidence (commands, test results, source references);
- proposed decisions (never silently accepted);
- risks, blockers, next suggested action;
- provider/model usage and duration.

Reject malformed output. Do not execute code, commands, URLs, file paths, or tool instructions embedded in free-form model text.

### Run states

`DRAFT -> READY -> RUNNING -> WAITING_FOR_INPUT | WAITING_FOR_APPROVAL | SUCCEEDED | FAILED | CANCELLED`

A run is successful only when all required acceptance checks have evidence and no required approval remains outstanding. Task-file status and runtime run state remain separate concepts.

## 6. Tool policy and execution safety

MVP starts with **no general-purpose shell or unrestricted MCP proxy**. Tool adapters are explicitly registered and operations are allowlisted by role, task, resource and environment.

Risk classes follow the existing policy:
- R0 read: task-scoped reads.
- W1 reversible write: scoped draft artifacts on a feature branch.
- W2 consequential write: approval before execution.
- D3 destructive/privileged: owner approval plus relevant technical/security review; MVP should deny these by default.

The first verification runner should support a small configured command registry (for example, repository validation scripts) with fixed executable, argument schema, working-directory boundary, timeout, output limit, and environment allowlist. Never concatenate model-generated strings into a shell command. Run in a disposable container or other isolated workspace before enabling untrusted repository execution.

Protect `.env`, credential files, SSH material, cloud credentials, and host filesystem paths. Secrets must not enter prompts, model context, task files, artifacts, or logs. Redact known secret patterns before persistence.

## 7. Persistence and artifacts

The runtime needs durable state independent of model conversation history.

Minimum records:
- `runs`: identity, initiator, scope, state, policy version, timestamps, budgets, final outcome;
- `steps`: role, objective, dependency, status, attempt count, provider/model, timestamps;
- `events`: append-only event ID, run/step IDs, event type, actor, timestamp, safe metadata;
- `approvals`: exact action/resource/preview hash, approver, decision, expiry;
- `artifacts`: logical reference, content hash, media type, size, storage location, classification.

Persist event before acknowledging a state transition. Keep large artifacts outside event rows and store content hashes/references. Apply retention and access controls. No secrets or unrestricted prompt dumps in audit records.

Technology choice for persistence is intentionally left to ADR-0002. A local-first MVP should favor low operational overhead but must support transactions and reliable restart/reconciliation.

## 8. Verification and self-correction loop

1. Agent proposes a change or artifact.
2. Schema/path/policy validation runs.
3. Configured tests, lint, or static checks run in the isolated workspace.
4. Failure output is minimized and sanitized, then returned to the responsible agent with a bounded repair budget.
5. Re-run the same checks.
6. If checks still fail or budget expires, stop and hand off with evidence.

The loop has hard limits: maximum repair attempts, model calls, tool calls, wall-clock duration, and spend. Passing tests is evidence, not proof of correctness; QA/reviewer approval remains separate.

## 9. Observability and budgets

Every event carries correlation identifiers: `runId`, `stepId`, `invocationId`, `toolCallId`.

Capture:
- run/step state transitions and durations;
- provider/model, request IDs, token usage and cost (actual or clearly marked estimate);
- tool operation, risk class, authorization result, duration and safe result category;
- approval request/decision and actor;
- verification command ID, exit code, duration and bounded output reference;
- retry, timeout, cancellation, recovery and policy-denial reasons.

Enforce per-run limits before each model/tool call. Stop and request owner authorization when a budget is exhausted. Do not log hidden reasoning or raw secrets.

## 10. MVP delivery phases

1. **Foundation:** TypeScript contracts, configuration validation, provider interface, fake provider, schema tests.
2. **Single-agent run:** sequential coordinator, one specialist invocation, run state, event log, CLI start/status/cancel.
3. **Policy and tools:** default-deny policy engine, one read-only adapter, constrained verification runner, approval records (no autonomous W2/D3 execution).
4. **Verification loop:** bounded test/repair cycles, evidence and final handoff.
5. **Provider portability:** second provider adapter and cross-provider contract tests.
6. **Hardening:** restart/reconciliation, idempotency, secret redaction, sandbox tests, cost ceilings and failure injection.

Do not add parallel execution until sequential recovery, isolation, and audit tests pass.

## 11. Proposed acceptance criteria

- A run can be started, inspected, cancelled, and recovered after process restart.
- Coordinator and specialist use validated, versioned contracts.
- At least one real provider and one fake provider implement the same interface.
- Provider-specific code is isolated; model ID is configurable and not hard-coded into workflow logic.
- Default-deny policy blocks ungranted tool calls and tests prove the denial.
- W2/D3 actions pause for approval or are denied; there is no agent self-approval path.
- Verification commands come only from a fixed allowlist and run in an isolated workspace.
- Every state transition, model call, tool decision, approval, and verification result has an audit event.
- Secrets are excluded/redacted from context, logs, and artifacts.
- A bounded verification failure can be returned to an agent for repair; retries and budgets are enforced.
- Documentation accurately states what is implemented and what remains a proposal.

## 12. Open decisions

See ADR-0002. No implementation should begin until the Product Owner accepts the runtime boundary, initial deployment mode, persistence choice, and provider/data policy. Security Reviewer must approve the threat model and sandbox/tool boundary before write-capable execution is enabled.
