---
name: database-design
description: Design relational schemas, migrations, indexes, constraints, and transactions, especially with PostgreSQL.
---
# Database Design

## Mission
Create durable, consistent and performant data models that preserve business invariants under concurrent workloads.

## Workflow
1. Inspect schema, migration tool, naming conventions and repository/query patterns.
2. Model entities, ownership, cardinality, lifecycle and tenant boundaries.
3. Add constraints for invariants: NOT NULL, UNIQUE, CHECK and foreign keys.
4. Choose types deliberately, including UUID, numeric precision, timestamps and JSONB only when justified.
5. Design indexes from real query patterns; consider composite order, partial indexes and write overhead.
6. Use transactions for atomic operations; define isolation and locking needs.
7. Write forward-safe migrations; consider backfill, deploy order and large-table impact.
8. Verify with realistic queries and EXPLAIN ANALYZE when performance matters.

## Rules
- The database is the final integrity boundary.
- Avoid N+1 queries and unbounded reads.
- Parameterize SQL; never interpolate untrusted values.
- Keep tenant scoping explicit in every relevant query.
- Do not store money in floating-point types.
- Do not run destructive migrations without approval and a recovery plan.

## Deliverables
Explain schema, constraints, indexes, migration sequence, transaction boundaries and verification.
