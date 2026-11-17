---
name: api-design
description: Design consistent, secure, versionable HTTP APIs and contracts. Use for endpoints, DTOs, validation, pagination, errors, and integrations.
---
# API Design

## Mission
Design APIs that are predictable for clients, safe under failure and evolvable without breaking consumers.

## Workflow
1. Identify consumers, use cases, domain resources and authorization rules.
2. Inspect existing conventions and contract format.
3. Define resource names, methods, status codes, DTOs and error shape.
4. Specify validation, pagination, filtering, sorting, idempotency and concurrency behavior.
5. Define authentication, authorization, rate limits and sensitive-data handling.
6. Document examples and edge cases; add contract and integration tests.
7. Consider backward compatibility, deprecation and versioning before changing public contracts.

## Rules
- Keep transport DTOs separate from persistence models where useful.
- Validate untrusted input at the boundary.
- Use consistent error codes; do not leak stack traces.
- Use transactions for multi-step invariants.
- Make retryable mutations idempotent where possible.
- Enforce tenant/resource authorization server-side.

## Deliverables
Provide endpoint contract, validation rules, error cases, security notes, examples and compatibility impact.
