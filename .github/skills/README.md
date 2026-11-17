# Reusable Agent Skills

A curated library of focused, reusable skills for this repository's AI agents.

An **agent** defines a role and responsibility. A **skill** provides specialist knowledge and a repeatable workflow for a task.

## How to use
- In supported GitHub Copilot environments, each skill is discoverable as a directory containing SKILL.md with YAML frontmatter.
- In a chat or agent builder without automatic repository skill discovery, provide the direct SKILL.md URL and ask the agent to read and follow it.
- Give an agent only the relevant skills for the task; avoid loading the entire library unnecessarily.

## Frontend

| Skill | When to use |
|---|---|
| [react-nextjs-development](./frontend/react-nextjs-development/SKILL.md) | Build production React, Next.js, and TypeScript applications. Use for UI features, routing, rendering, data fetching, forms, state, and component architecture. |
| [desktop-web-apps](./frontend/desktop-web-apps/SKILL.md) | Build complex desktop-first web applications, dashboards, admin panels, and responsive SPAs. Use for multi-page business software and dense data interfaces. |
| [telegram-mini-apps](./frontend/telegram-mini-apps/SKILL.md) | Build Telegram Mini Apps using the Telegram WebApp platform and SDK. Use for web products embedded in Telegram chats, bots, and launch flows. |
| [pwa-development](./frontend/pwa-development/SKILL.md) | Create installable Progressive Web Apps with manifests, service workers, offline behavior, and push notifications. |
| [offline-first-architecture](./frontend/offline-first-architecture/SKILL.md) | Design local-first and offline-capable applications with durable local storage, synchronization, and conflict handling. |
| [threejs-webgl-development](./frontend/threejs-webgl-development/SKILL.md) | Create interactive 3D web experiences with Three.js, React Three Fiber, WebGL, shaders, and optimized 3D assets. |
| [frontend-performance](./frontend/frontend-performance/SKILL.md) | Diagnose and improve frontend loading, rendering, runtime, and Core Web Vitals. |
| [ui-ux-accessibility](./frontend/ui-ux-accessibility/SKILL.md) | Design accessible, consistent interfaces and interaction patterns. Use for UI reviews, design systems, forms, navigation, and WCAG improvements. |
| [frontend-testing](./frontend/frontend-testing/SKILL.md) | Build reliable frontend testing with unit, component, integration, visual, and end-to-end tests. |

## Backend & Architecture

| Skill | When to use |
|---|---|
| [api-design](./backend/api-design/SKILL.md) | Design consistent, secure, versionable HTTP APIs and contracts. Use for endpoints, DTOs, validation, pagination, errors, and integrations. |
| [database-design](./backend/database-design/SKILL.md) | Design relational schemas, migrations, indexes, constraints, and transactions, especially with PostgreSQL. |
| [nodejs-backend](./backend/nodejs-backend/SKILL.md) | Implement production Node.js and TypeScript backend services with clear layering, dependency boundaries, validation, and observability. |
| [authentication-authorization](./backend/authentication-authorization/SKILL.md) | Implement authentication, sessions, permissions, RBAC, OAuth/OIDC, and secure identity flows. |
| [redis-caching](./backend/redis-caching/SKILL.md) | Use Redis for caching, rate limiting, distributed coordination, and ephemeral data. |
| [realtime-websocket](./backend/realtime-websocket/SKILL.md) | Design real-time communication with WebSockets, SSE, and polling for live dashboards, POS updates, and notifications. |

## Backend & Architecture

| Skill | When to use |
|---|---|
| [system-design](./architecture/system-design/SKILL.md) | Design scalable and resilient application architectures. Use for new systems, major refactors, capacity planning, and architecture reviews. |
| [docker-cicd](./architecture/docker-cicd/SKILL.md) | Containerize applications and build reliable CI/CD pipelines using Docker, Compose, GitHub Actions, and deployment automation. |

## AI Engineering

| Skill | When to use |
|---|---|
| [prompt-engineering](./ai/prompt-engineering/SKILL.md) | Design, evaluate, and maintain prompts for reliable LLM behavior, structured outputs, tool policies, and prompt debugging. |
| [llm-api-integration](./ai/llm-api-integration/SKILL.md) | Integrate hosted or local LLM APIs into production applications with streaming, structured outputs, retries, and cost controls. |
| [tool-calling](./ai/tool-calling/SKILL.md) | Design safe tool/function calling so LLMs can interact with APIs and business systems. |
| [agent-development](./ai/agent-development/SKILL.md) | Build reliable tool-using AI agents with clear state, bounded autonomy, memory policy, and human oversight. |
| [rag-vector-search](./ai/rag-vector-search/SKILL.md) | Build retrieval-augmented generation systems over documents and structured knowledge. |
| [llm-evaluation](./ai/llm-evaluation/SKILL.md) | Evaluate LLM applications and agents with repeatable datasets, rubrics, automated checks, and human review. |
| [ai-observability-cost](./ai/ai-observability-cost/SKILL.md) | Instrument AI features and agents for traces, quality, latency, token usage, and spend. |

## Security

| Skill | When to use |
|---|---|
| [security-review](./security/security-review/SKILL.md) | Review application and AI-agent security, identify threats, and recommend mitigations. |

## Authoring conventions
- Directory name and frontmatter name must match.
- Keep descriptions specific about capability and trigger conditions.
- Explain workflow, quality rules, failure modes and expected deliverables.
- Prefer project conventions over imposing a new stack.
- Never claim checks passed unless they were actually run.
- Keep secrets and private data out of skill files.

## Direct-link prompt
Read this skill completely and apply its workflow and rules to the task:

[SKILL.md URL]

Also inspect the target repository's instructions, architecture and existing conventions before making changes.
