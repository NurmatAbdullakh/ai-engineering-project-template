# Reusable agent profiles

These 11 role profiles are GitHub Copilot custom-agent definitions and the canonical role descriptions for the workspace's persistent chat-based team. The profiles do not create or coordinate ChatGPT conversations automatically.

## Available agents

| Agent | Profile |
|---|---|
| Product Owner | [product-owner.agent.md](./product-owner.agent.md) |
| Project Manager | [project-manager.agent.md](./project-manager.agent.md) |
| Business Analyst | [business-analyst.agent.md](./business-analyst.agent.md) |
| UI/UX Designer | [ui-ux-designer.agent.md](./ui-ux-designer.agent.md) |
| Software Architect | [architect.agent.md](./architect.agent.md) |
| Backend Engineer | [backend-engineer.agent.md](./backend-engineer.agent.md) |
| Frontend Engineer | [frontend-engineer.agent.md](./frontend-engineer.agent.md) |
| QA Engineer | [qa-engineer.agent.md](./qa-engineer.agent.md) |
| DevOps Engineer | [devops-engineer.agent.md](./devops-engineer.agent.md) |
| Security Reviewer | [security-reviewer.agent.md](./security-reviewer.agent.md) |
| Team Lead | [team-lead.agent.md](./team-lead.agent.md) |

## Using a profile in GitHub Copilot

These files are stored in `.github/agents/` and can be selected as GitHub Copilot custom agents after they are merged into the repository's default branch. Copilot's agent selection behavior is specific to the supported GitHub Copilot environment.

## Using a profile in a persistent ChatGPT conversation

Create one dedicated ChatGPT conversation per role and keep using that conversation for future tasks in the same project.

1. Open the role profile above and copy its GitHub URL.
2. Open [the persistent chat operating model](../../docs/team/CHAT_BASED_AGENT_OPERATING_MODEL.md).
3. Copy the [shared bootstrap prompt](../../docs/team/templates/CHAT_AGENT_BOOTSTRAP.md) and fill in the workspace repository URL, target product repository URL, and role profile URL.
4. Use the [agent-to-skill matrix](../../docs/team/AGENT_SKILL_MATRIX.md) to load only task-relevant skills.
5. For later tasks in that same chat, send the Task ID/URL and expected handoff; do not repeat the full bootstrap.

If a chat cannot access GitHub or a referenced file, provide the file contents or ask the chat to request them. It must report inaccessible sources honestly. GitHub task files, decisions, PRs, and handoffs remain the cross-chat source of truth; ChatGPT conversations do not automatically share history or route tasks.
