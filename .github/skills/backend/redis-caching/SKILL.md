---
name: redis-caching
description: Use Redis for caching, rate limiting, distributed coordination, and ephemeral data.
---
# Redis & Caching

## Mission
Use Redis deliberately for performance and coordination without making cache state a hidden source of truth.

## Workflow
1. Identify the measured bottleneck and consistency requirements.
2. Choose cache-aside, read-through, write-through or write-behind only where appropriate.
3. Define key schema, namespace, TTL, serialization, invalidation and versioning.
4. Protect against cache stampedes with coalescing, jitter, locks or stale-while-revalidate.
5. Define behavior when Redis is unavailable based on operation risk.
6. For distributed locks, define ownership tokens, expiry and safe release.
7. Monitor hit ratio, latency, evictions, memory, connections and errors.

## Rules
- Durable storage remains source of truth unless architecture explicitly says otherwise.
- Never cache sensitive data without access and invalidation analysis.
- Avoid unbounded keys and missing TTLs for ephemeral data.
- Rate limits must account for distributed instances and identity/IP policy.
- Do not use Redis primitives as a substitute for domain design.

## Deliverables
Provide key design, TTL/invalidation policy, failure mode, consistency trade-offs and metrics.
