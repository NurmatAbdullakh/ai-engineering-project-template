---
name: nodejs-backend
description: Implement production Node.js and TypeScript backend services with clear layering, dependency boundaries, validation, and observability.
---
# Node.js Backend Engineering

## Mission
Build maintainable Node.js/TypeScript services with explicit boundaries, reliable I/O and production-grade error handling.

## Workflow
1. Inspect runtime, package manager, framework, module system, scripts and architecture.
2. Trace request flow: route → middleware → controller → service/use case → repository/provider.
3. Define typed contracts and validate untrusted input at boundaries.
4. Keep controllers thin; put business rules in services and persistence in repositories.
5. Inject external dependencies where it improves testability.
6. Handle async errors, timeouts, cancellation, retries and resource cleanup.
7. Add structured logs, metrics and tracing consistent with the project.
8. Run typecheck, lint, tests and build.

## Rules
- Avoid hidden global mutable state and unnecessary abstractions.
- Use parameterized queries and explicit transaction ownership.
- Do not log secrets, tokens or sensitive payloads.
- Set timeouts for outbound calls.
- Distinguish operational errors from programmer errors.
- Respect graceful shutdown.
- Never swallow errors or return fake success.

## Deliverables
Describe request flow, changed layers, dependency choices, error paths, observability and checks run.
