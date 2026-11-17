# Persistent chat-based AI Engineering Team

**Status:** Current operating model  
**Owner:** Product Owner  
**Source of truth:** This repository and the target product repository's own documentation

## 1. Purpose

The team is operated through persistent, role-specific ChatGPT conversations. The user speaks primarily with the Product Owner chat. Work then moves through specialist chats in layers. Each specialist chat is a durable working space for one role; GitHub documents, not chat history, are the shared source of truth.

This is a human-coordinated workflow. It is not an automated multi-agent runtime, and one chat cannot assume another chat has run or completed work unless its output is available in GitHub or explicitly supplied.

## 2. Operating model

- One persistent chat per role. Keep using that role's chat for new tasks in the same project.
- Each chat has one primary role and follows the matching profile in `.github/agents/`.
- The workspace repository provides shared governance, role definitions, and reusable skills.
- The target product repository provides the actual product context, source code, architecture, task files, and delivery evidence.
- GitHub is the cross-chat memory and handoff mechanism. Chat history is helpful working context, not an authoritative record.
- The Product Owner retains product decisions and final acceptance. The user coordinates which chat receives the next handoff.

## 3. Role roster and normal sequence

The normal product-to-delivery path is:

1. **Product Owner** — clarify the problem, goals, scope, priorities, and product acceptance; produce or approve the product brief/PRD.
2. **Business Analyst** — turn approved product intent into detailed requirements, business rules, user flows, and testable acceptance criteria.
3. **Project Manager** — decompose approved requirements into epics and independently verifiable tasks, dependencies, owners, and execution order.
4. **UI/UX Designer** — produce interaction flows and design handoff when the feature requires a user interface.
5. **Software Architect** — define technical boundaries, data model, API contracts, integration points, and ADRs where required.
6. **Backend Engineer** — implement backend tasks and backend tests.
7. **Frontend Engineer** — implement frontend tasks and integrate approved API/design contracts.
8. **QA Engineer** — verify acceptance criteria, regression risks, and evidence.
9. **Security Reviewer** — review security-sensitive changes and relevant threat boundaries.
10. **DevOps Engineer** — handle CI/CD and operational readiness when the task requires it.
11. **Team Lead** — review cross-layer consistency and integration handoff when multiple roles contribute.

This is a routing guide, not a requirement to invoke every role for every feature. The Project Manager or Product Owner may omit irrelevant layers with a recorded reason. Architecture and design work may happen in parallel only when their inputs are sufficiently independent; implementation must respect agreed contracts.

## 4. Starting a persistent role chat

For each role, create one ChatGPT conversation and keep it for that role/project. At the beginning of the conversation:

1. Provide the workspace repository URL.
2. Provide the target product repository URL. Do not assume it is the workspace repository.
3. Provide the role profile URL from `.github/agents/`.
4. Paste the shared bootstrap prompt from [CHAT_AGENT_BOOTSTRAP.md](./templates/CHAT_AGENT_BOOTSTRAP.md), filling in the role and repository URLs.
5. Ask the chat to confirm which instructions and repository documents it could actually read.
6. Keep the conversation dedicated to that role. Do not turn one role chat into a general-purpose implementation chat.

If the chat cannot access GitHub or a referenced file, it must say so and request the file or relevant excerpt. It must never claim to have read inaccessible content.

## 5. Task intake protocol

For every new task, the user supplies the task ID or GitHub task URL and the expected handoff.

The receiving chat must:

1. Read the task file from the target product repository.
2. Read the target repository's `AGENTS.md`, README, project context, workflow, relevant architecture/ADR, and related code as appropriate.
3. Read the workspace role profile and shared rules. Load only skills relevant to the task using the skill matrix.
4. Summarize the task objective, acceptance criteria, dependencies, constraints, and unknowns.
5. Check whether upstream outputs referenced by the task exist and are approved.
6. Ask one focused question only if a missing decision blocks safe progress; otherwise state assumptions and proceed within scope.
7. Confirm the work boundary before making changes. Create or use a feature branch and keep work limited to the assigned task.

A chat must not silently change scope, invent approval, or treat a prior chat's unverified statement as evidence.

## 6. Work and completion protocol

- Follow the target repository's instructions and conventions. Workspace rules do not override a target repository's accepted architecture or security policy; conflicts must be raised to the Product Owner.
- Keep implementation, tests, docs, task status, and handoff aligned in one reviewable change where practical.
- Run the checks relevant to the actual repository. Report exact commands and outcomes. Clearly mark checks that could not be run.
- Do not merge, deploy, run destructive database operations, alter production secrets, or perform other gated actions without explicit approval for the exact action.
- Do not claim another role completed work unless its artifact or evidence is available.
- If blocked, update the task with the blocker, evidence, responsible owner, and next action.

## 7. Required handoff

Every completed role task must leave a durable handoff in the target repository task or linked artifact. At minimum include:

- Task ID and objective.
- Deliverables and changed files/links.
- Decisions and assumptions.
- Verification commands and results; tests not run.
- Risks, open questions, and blockers.
- Explicit next role/owner and the exact next action.
- Status recommendation (for example, IN_REVIEW or BLOCKED).

The receiving chat must acknowledge the handoff and check that referenced artifacts exist. A handoff is not accepted solely because it was mentioned in chat.

## 8. Context recovery and continuity

When a conversation becomes too long, a new chat is required, or context may have been lost:

1. Re-read the role profile and shared bootstrap instructions.
2. Read the current task and its latest handoff from GitHub.
3. Read linked upstream artifacts and relevant accepted ADRs.
4. Inspect the current branch/PR state and relevant changed files.
5. Summarize the verified current state before continuing.

Never rely on a chat summary when GitHub contains a newer task, decision, or implementation state.

## 9. Current limitations

- ChatGPT conversations are not automatically synchronized with one another.
- No automatic task routing, context transfer, shared chat memory, or central runtime is implemented.
- The user manually opens the relevant persistent chat and supplies the task/handoff reference.
- The repository's `.github/agents/` profiles are also GitHub Copilot custom-agent definitions. ChatGPT use requires the bootstrap process; GitHub Copilot does not automatically configure ChatGPT conversations.
- The future Agent Runtime design is a separate proposal and is not part of this operating model's current execution.

## 10. Definition of ready for a specialist chat

A task is ready when the receiving chat has:
- A stable task ID and accessible task file.
- A clear objective and testable acceptance criteria.
- The required upstream documents/contracts or explicit permission to create them.
- A named owner and next handoff.
- A target repository and branch/change boundary.
- Known approval gates and relevant skills identified.
