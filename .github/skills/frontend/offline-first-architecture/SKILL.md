---
name: offline-first-architecture
description: Design local-first and offline-capable applications with durable local storage, synchronization, and conflict handling.
---
# Offline-First Architecture

## Mission
Make core workflows reliable during disconnection and synchronize safely when connectivity returns. Offline-first is a data architecture, not just a cached page.

## Workflow
1. Identify workflows that must work offline and define explicit guarantees.
2. Classify data: reference data, user edits, transactional commands, sensitive data and ephemeral state.
3. Choose storage deliberately: IndexedDB for structured browser data; consider Dexie if compatible.
4. Define local schema, versioning, migrations, indexes, retention and quota behavior.
5. Record mutations as durable pending operations or an outbox with stable operation IDs and idempotency keys.
6. Design synchronization: retry with backoff, batching, ordering, auth refresh and partial failure.
7. Define conflict policy per entity: server-wins, client-wins, field merge, version checks or user resolution.
8. Test offline edits, reconnect, duplicate delivery, app restart, expired sessions and concurrent edits.

## Rules
- Do not promise offline support for workflows requiring server authorization or real-time financial/inventory truth.
- Make sync state visible: pending, syncing, synced and failed.
- Ensure retryable server commands are idempotent.
- Never silently discard local user work.
- Protect local sensitive data and define logout/data-clear behavior.
- Distinguish optimistic UI from confirmed server state.

## Deliverables
Provide offline scope, data flow, storage schema, sync protocol, conflict policy, recovery and tests.
