# ADR-0002: Agent runtime MVP technology and deployment

- **Status:** Proposed — Product Owner approval required
- **Date:** 2026-11-17
- **Decision owners:** Product Owner, Software Architect
- **Related task:** TASK-AIWS-007
- **Related design:** [Agent Runtime MVP Technical Design](../AGENT_RUNTIME_MVP_DESIGN.md)
- **Supersedes:** None

## Context

ADR-0001 recommends a provider-neutral coordinator-first workflow but intentionally leaves runtime technology open. The workspace is currently a Markdown-first repository with Node.js automation scripts and GitHub Actions. No application runtime, database, hosting target, or agent provider has been selected.

The owner has frontend experience with TypeScript/React/Next.js and is developing Node.js backend skills. The runtime should be small enough to understand and operate, while preserving testability, durable state, policy enforcement, and vendor portability.

## Decision proposal

Adopt the following as the recommended MVP baseline, subject to owner approval:

1. **Language/runtime:** Node.js + TypeScript, using a modular monolith with clear ports/adapters. This aligns with the existing repository automation and avoids introducing a second runtime.
2. **Initial interface:** Local CLI first. Do not expose a public HTTP service until authentication, tenancy, network boundary, and operational requirements are designed.
3. **Workflow:** Sequential coordinator with one active specialist step at a time. No parallel agent writes in MVP.
4. **Model integration:** Provider-neutral interface; implement one approved provider first, then a second adapter as a portability test. Anthropic, OpenAI, and Google are candidate providers, not pre-approved defaults.
5. **Persistence:** Select a transactional local-first store for run state and append-only events. SQLite is the leading MVP candidate; Postgres is a future shared/multi-user deployment option. Final selection requires owner approval after restart/concurrency requirements are confirmed.
6. **Tool execution:** Explicit adapter registry and default-deny policy. No unrestricted MCP proxy, arbitrary shell tool, or direct agent credential access.
7. **Verification:** Fixed command registry and disposable isolated execution workspace. No host secrets or unrestricted host filesystem mounts.
8. **Delivery:** Feature-branch PRs and existing repository validation. No autonomous merge or production deployment.

## Alternatives considered

### A. Provider-specific framework and runtime

Choose a single provider's SDK and framework as the center of the system.

**Pros:** Fast initial integration; can use provider-native features quickly.

**Cons:** Couples orchestration semantics, tool calling, session state, and future migration to one vendor; makes multi-provider policy and portability harder.

**Assessment:** Not recommended as the core architecture. Provider SDKs may be used inside adapters.

### B. Vendor-neutral TypeScript modular monolith (recommended)

Define internal contracts for model providers, tools, policy, approvals, persistence, and verification. Use provider SDKs only in adapters.

**Pros:** Fits existing Node.js tooling; easy to test with fake adapters; supports multiple model vendors; avoids premature distributed services.

**Cons:** Requires maintaining adapter contracts and handling provider capability differences explicitly.

**Assessment:** Recommended for the MVP.

### C. Distributed microservices from the start

Separate coordinator, model gateway, tool gateway, event store, and worker services.

**Pros:** Independent scaling and deployment boundaries.

**Cons:** Excess operational complexity before workload, concurrency, and reliability requirements are known.

**Assessment:** Defer until measured needs justify it.

### D. Hosted service first

Deploy a shared orchestration API immediately.

**Pros:** Remote execution, shared state, easier team access.

**Cons:** Requires authentication, tenant isolation, secrets management, network controls, cost governance, and production operations before the MVP value is proven.

**Assessment:** Defer; local CLI is safer and faster for initial validation.

## Consequences

### Positive
- TypeScript contracts can be shared across coordinator, adapters, validators, and CLI.
- Local-first execution limits exposure while the permission model is validated.
- Provider neutrality avoids locking task workflows to Claude Code or any one model vendor.
- Modular monolith keeps the first deployment and debugging surface small.
- Fake providers and tool adapters enable deterministic tests without paid model calls.

### Trade-offs and risks
- Local execution still needs a sandbox; a CLI alone does not isolate untrusted repository code.
- SQLite is suitable only for an initial local/single-user mode, not assumed to support a multi-host distributed deployment.
- Provider-neutral contracts cannot erase differences in structured outputs, tool calling, context windows, streaming, or usage reporting.
- The runtime will initially be manually operated; no unattended background agent service is implied.

## Approval checklist

- [ ] Product Owner approves Node.js + TypeScript modular monolith.
- [ ] Product Owner approves local CLI as the first deployment interface.
- [ ] Product Owner approves sequential-only execution for MVP.
- [ ] Product Owner approves SQLite as the local persistence candidate, or selects an alternative.
- [ ] Product Owner selects the first model provider and confirms data handling constraints.
- [ ] Security Reviewer approves the threat model, sandbox approach, secret boundary, and tool permission enforcement before any write-capable agent tool is enabled.

Until these items are approved, this ADR remains a proposal. Do not treat candidate providers, SQLite, or the local CLI as already selected or implemented.
