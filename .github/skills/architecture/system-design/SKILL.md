---
name: system-design
description: Design scalable and resilient application architectures. Use for new systems, major refactors, capacity planning, and architecture reviews.
---
# System Design

## Mission
Translate product requirements into a clear architecture with explicit trade-offs, failure boundaries and operational needs.

## Workflow
1. Clarify functional and non-functional requirements, constraints and out-of-scope items.
2. Estimate users, requests, payloads, data growth, concurrency and latency targets.
3. Draw system context and key request/data flows.
4. Identify domain boundaries, ownership, synchronous/asynchronous work and consistency needs.
5. Evaluate database, cache, queue, search, storage and communication choices.
6. Analyze bottlenecks, single points of failure, security boundaries and degraded modes.
7. Plan observability, deployment, migrations, backup/restore and disaster recovery.
8. Record decisions and revisit assumptions as evidence changes.

## Rules
- Prefer the simplest architecture that satisfies requirements.
- Do not introduce microservices, queues or locks without concrete need.
- State consistency and availability trade-offs explicitly.
- Include failure scenarios, retries, idempotency and timeouts.
- Design tenant isolation and authorization into data flows.

## Deliverables
Provide requirements, architecture flow, key decisions, trade-offs, risks and phased implementation plan.
