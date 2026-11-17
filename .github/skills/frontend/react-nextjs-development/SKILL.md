---
name: react-nextjs-development
description: Build production React, Next.js, and TypeScript applications. Use for UI features, routing, rendering, data fetching, forms, state, and component architecture.
---
# React & Next.js Development

## Mission
Deliver maintainable, accessible, production-ready React and Next.js features using TypeScript. Treat the repository as the source of truth; do not impose a new stack without a reason.

## Workflow
1. Inspect package scripts, framework version, routing mode, components, styling, data fetching and state patterns.
2. Trace the user flow and API contract before changing UI.
3. Define typed boundaries for props, API DTOs, form schemas and domain models.
4. Prefer small composable components and feature-oriented organization.
5. In Next.js, choose Server and Client Components deliberately; keep client boundaries narrow and follow the existing router convention.
6. Handle loading, empty, error, success and permission states.
7. Run lint, typecheck, tests and production build where available.

## Rules
- Avoid any; validate untrusted API data at runtime when appropriate.
- Do not duplicate server state in global client state without a clear need.
- Use semantic HTML, keyboard support and accessible labels.
- Avoid unnecessary effects; derive values during render where possible.
- Keep secrets and privileged operations on the server.
- Respect existing design tokens and component libraries.

## Deliverables
Summarize changed files, architectural decisions, edge cases, commands run and remaining risks. Never claim checks passed unless executed.
