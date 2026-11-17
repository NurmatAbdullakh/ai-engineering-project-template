---
id: TASK-AIWS-008
title: Establish persistent ChatGPT role chat setup
feature: persistent-chat-agent-setup
status: IN_REVIEW
priority: CRITICAL
assignee: TEAM_LEAD
sprint: SPRINT-AIWS-003
estimate: 5
depends_on: [TASK-AIWS-001, TASK-AIWS-002, TASK-AIWS-003, TASK-AIWS-005, TASK-AIWS-006]
labels: [agents, chat-workflow, documentation]
---

## Objective
Make the existing specialist role profiles practical to use as persistent, separate ChatGPT conversations, with a repeatable setup, shared context, skill selection, task intake, and durable handoff protocol.

## Acceptance criteria
- [x] The current human-coordinated, chat-based operating model is explicit and distinguished from a future automated runtime.
- [x] All 11 persistent role chats have a reusable role-profile reference.
- [x] A copy-ready shared bootstrap prompt explains repository context, role boundaries, and access honesty.
- [x] A role-to-skill matrix identifies default and conditional skills without claiming unavailable skills are installed.
- [x] Task intake, execution, handoff, and context recovery are documented.
- [x] Workspace context and workflow reflect the current operating model.
- [ ] Product Owner reviews and accepts this setup.

## Deliverables
- `docs/team/CHAT_BASED_AGENT_OPERATING_MODEL.md`
- `docs/team/AGENT_SKILL_MATRIX.md`
- `docs/team/templates/CHAT_AGENT_BOOTSTRAP.md`
- Updated `.github/agents/README.md`, `docs/team/PROJECT_CONTEXT.md`, and `docs/team/WORKFLOW.md`.

## Handoff
- **Task ID:** TASK-AIWS-008
- **Objective:** Establish one persistent chat per specialist role and a GitHub-backed process for transferring work between them.
- **Evidence:** See the three deliverables above and the updated team instructions.
- **Validation:** Documentation links, role roster, bootstrap placeholders, and skill paths reviewed against the current repository files. GitHub Actions validation/build must be checked on the PR; local execution is not claimed.
- **Risks/blockers:** ChatGPT conversations do not automatically share history or route tasks. The user must manually open the appropriate role chat and provide task/handoff references. Repository access depends on the connected tools and permissions available in each chat.
- **Next owner:** Product Owner.
- **Next step:** Review the operating model and bootstrap prompt; then create/configure the persistent role chats using the role profile links and bootstrap template.
