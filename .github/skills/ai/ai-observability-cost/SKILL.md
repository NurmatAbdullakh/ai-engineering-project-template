---
name: ai-observability-cost
description: Instrument AI features and agents for traces, quality, latency, token usage, and spend. Use when operating LLM products or optimizing model costs.
---
# AI Observability & Cost Optimization

## Mission
Make AI behavior, reliability and unit economics visible without exposing sensitive user content.

## Workflow
1. Define metrics: task success, tool success, latency, tokens and cost per completed task.
2. Trace each request across orchestration, model calls, retrieval and tools with correlation IDs.
3. Record provider/model, prompt version, token counts, retries, duration and outcome.
4. Redact or minimize prompts, tool arguments and outputs before logging.
5. Build dashboards and alerts for spend spikes, latency, errors and quality regressions.
6. Optimize with caching, context reduction, batching and model routing only after evaluation.
7. Add budgets, quotas and graceful degradation per user, tenant or feature.
8. Review unit economics using successful task completion, not only cost per model call.

## Rules
- Never log API keys, credentials or unrestricted personal data.
- Detect runaway retries and agent loops.
- Do not silently degrade critical task quality to reduce cost.
- Make budget exhaustion behavior explicit.
- Separate provider billing estimates from verified invoices.

## Deliverables
Provide trace design, metric definitions, privacy controls, budget policy and measured optimization impact.
