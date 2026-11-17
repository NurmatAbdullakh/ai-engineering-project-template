---
name: agent-development
description: Build reliable tool-using AI agents with explicit state, bounded autonomy, memory policy, and human oversight. Use for assistants and workflow automation.
---
# AI Agent Development

## Mission
Build agents that complete useful tasks safely and predictably, not merely chat interfaces with a model call.

## Workflow
1. Define the agent's job, boundaries, success criteria and escalation cases.
2. Map workflow states; decide which steps require deterministic code versus model reasoning.
3. Give the agent a small, typed toolset with server-side authorization.
4. Define context assembly, memory scope, retention and data minimization.
5. Bound autonomy with step limits, timeouts, token budgets, retries and approval gates.
6. Make long-running work resumable and observable; persist state outside process memory.
7. Add idempotency and compensation for retryable or partially failed actions.
8. Evaluate task success, unsafe actions, tool selection, latency and cost.

## Architecture principles
- Prefer workflow/state-machine orchestration for predictable business processes.
- Use agent loops only where dynamic reasoning adds value.
- Keep business rules and permissions in application services.
- Separate planning from execution for high-impact actions.
- Provide human handoff and clear action summaries.
- Design for duplicate messages, interrupted runs and provider outages.

## Deliverables
Provide agent contract, state flow, tools, memory policy, safety gates, evaluation and operational metrics.
