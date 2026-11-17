---
name: llm-api-integration
description: Integrate hosted or local LLM APIs into production applications with streaming, structured outputs, retries, and cost controls.
---
# LLM API Integration

## Mission
Integrate language models behind reliable application boundaries with provider-aware behavior and measurable quality/cost.

## Workflow
1. Define use case, quality target, latency budget, privacy constraints and request volume.
2. Inspect provider SDKs, models, structured output, streaming and tool-call capabilities.
3. Hide provider-specific details behind a small adapter when multi-provider flexibility is needed.
4. Validate input/output schemas; handle refusal, truncation, malformed output and context limits.
5. Implement timeouts, bounded retries, rate-limit handling and cancellation.
6. Stream safely; define disconnect and partial-response behavior.
7. Track model, prompt version, tokens, latency, errors and cost without logging sensitive content.
8. Evaluate model changes before rollout.

## Rules
- Keep API keys server-side.
- Do not retry non-idempotent downstream actions blindly.
- Set token budgets and maximum output sizes.
- Use the smallest model that meets quality, verified by evaluation.
- Avoid sending unnecessary personal or confidential data.
- Provide fallbacks and graceful degradation.

## Deliverables
Describe provider abstraction, model choice, schemas, resilience, privacy, observability and cost controls.
