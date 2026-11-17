---
name: llm-evaluation
description: Evaluate LLM applications and agents with repeatable datasets, rubrics, automated checks, and human review. Use before model or prompt changes and for regression testing.
---
# LLM Evaluation

## Mission
Replace intuition-only iteration with repeatable evidence about quality, safety, reliability and cost.

## Workflow
1. Define task-specific success criteria and failure severity.
2. Build a representative dataset, including edge and adversarial inputs.
3. Use deterministic checks for schemas, tool arguments, permissions and exact constraints.
4. Use rubric-based model judging only with calibration and human spot checks.
5. Evaluate retrieval, tool selection, final answer and end-to-end task completion separately.
6. Compare baseline and candidate using identical inputs and versioned prompts/models.
7. Track latency, tokens, cost, failure rate and variance.
8. Gate releases on critical safety and correctness thresholds.

## Rules
- Do not treat one aggregate score as sufficient.
- Keep test data representative and protect private data.
- Validate judge reliability and watch for bias.
- Include negative and refusal cases.
- Record model, prompt, tool and dataset versions.
- Investigate regressions by category.

## Deliverables
Provide dataset, metrics/rubric, baseline comparison, failure analysis and release recommendation.
