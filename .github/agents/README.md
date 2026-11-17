# Reusable agent profiles

These profiles are repository-level GitHub Copilot custom agents. Each role is independent; they do not automatically operate as a coordinated team.

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

## Using a profile in a new chat

1. Open the profile file and copy its GitHub URL.
2. In a chat or agent builder that can access GitHub, provide the URL and ask it to read the full file and follow its role instructions.
3. Also provide the target project's repository URL. Ask the agent to inspect that project's instructions and documentation before making changes.

If using GitHub Copilot custom agents, these files are stored in `.github/agents/` and can be selected after the changes are merged into the repository's default branch.