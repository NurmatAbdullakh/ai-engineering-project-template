---
name: frontend-testing
description: Build reliable frontend testing with unit, component, integration, visual, and end-to-end tests.
---
# Frontend Testing

## Mission
Test user-visible behavior at the right level while keeping tests deterministic and maintainable.

## Workflow
1. Inspect test runner, conventions, fixtures, mocks and CI.
2. Identify risk and critical journeys; prioritize auth, payments, permissions, data mutation and offline sync.
3. Use unit tests for pure logic, component tests for UI behavior, integration tests for connected features and E2E for critical flows.
4. Prefer accessible queries and user interactions over implementation-detail selectors.
5. Control network, time, randomness and browser state explicitly.
6. Add regression tests for reproduced bugs.
7. Run focused tests, then broader suite and type/lint checks.

## Special cases
- Test loading, empty, error, retry and permission states.
- Test PWA install/update/offline behavior in supported browsers.
- Test offline sync replay, duplicate operations and conflicts.
- Test Telegram bridge behind a mockable adapter, then verify real clients.
- For 3D, test accessible controls and core interaction; use visual/performance checks for rendering.

## Rules
- Avoid brittle snapshots as the only coverage.
- Do not mock the behavior under test.
- Never weaken assertions just to pass.
- Keep test data isolated and clean up.

## Deliverables
Summarize scenarios, coverage rationale, commands, results and known gaps.
