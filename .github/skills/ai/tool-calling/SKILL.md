---
name: tool-calling
description: Design safe LLM tool and function calling for APIs and business systems. Use when agents read data, create records, update prices, send messages, or trigger workflows.
---
# Tool Calling

## Mission
Expose narrowly scoped, validated application capabilities to an LLM without giving it unrestricted system access.

## Workflow
1. Define user intents and the smallest toolset required.
2. Give each tool a clear name, purpose, typed input schema and bounded output.
3. Validate arguments server-side; resolve identity and tenant from authenticated context, never model arguments.
4. Separate read tools from write tools and classify action risk.
5. Require confirmation for consequential, irreversible or externally visible actions when appropriate.
6. Enforce authorization, rate limits, idempotency and audit logging inside tool implementations.
7. Return concise structured results and actionable errors.
8. Test malformed arguments, prompt injection, cross-tenant attempts, duplicate calls and partial failures.

## Rules
- The model proposes; trusted application code authorizes and executes.
- Never expose raw SQL, shell, unrestricted HTTP or generic admin tools.
- Do not trust model-generated user IDs, tenant IDs, prices or permission claims.
- Make destructive actions narrow and reversible where possible.
- Treat tool output as untrusted context.
- Log actor, tool, target and outcome while protecting sensitive data.

## Deliverables
Provide tool catalog, schemas, risk classification, authorization/confirmation rules and adversarial tests.
