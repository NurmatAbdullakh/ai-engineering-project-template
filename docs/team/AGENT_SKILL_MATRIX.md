# Agent-to-skill matrix

Use this matrix to load specialist guidance into a persistent role chat. Agent profiles define responsibility; skills provide task-specific methods. A role should load only the skills relevant to the assigned task, not the entire library.

Skill paths are relative to `.github/skills/`. If a referenced skill is unavailable in the target environment, report that limitation and follow the repository's documented conventions instead.

| Agent role | Default skills | Conditional skills |
|---|---|---|
| Product Owner | No dedicated product-management skill currently exists. Use the Product Owner profile and workspace workflow. | `ai/prompt-engineering` only when designing or evaluating prompts/agent instructions. |
| Business Analyst | No dedicated business-analysis skill currently exists. Use the Business Analyst profile and approved product requirements. | `ai/prompt-engineering` only when requirements concern LLM prompts or agent behavior. |
| Project Manager | No dedicated project-management skill currently exists. Use the Project Manager profile, task template, and workflow. | `ai/llm-evaluation` when planning evaluation work for AI features. |
| UI/UX Designer | `frontend/ui-ux-accessibility` | `frontend/desktop-web-apps` for dense business applications; `frontend/react-nextjs-development` when design constraints depend on a React/Next.js implementation. |
| Software Architect | `architecture/system-design` | `backend/api-design`, `backend/database-design`, `architecture/docker-cicd`, `ai/agent-development`, and `security/security-review` as relevant to the architecture scope. |
| Backend Engineer | `backend/nodejs-backend` | `backend/api-design`, `backend/database-design`, `backend/authentication-authorization`, `backend/redis-caching`, `backend/realtime-websocket`, `architecture/docker-cicd`, `ai/tool-calling`, `ai/llm-api-integration` as relevant. |
| Frontend Engineer | `frontend/react-nextjs-development` | `frontend/desktop-web-apps`, `frontend/telegram-mini-apps`, `frontend/pwa-development`, `frontend/offline-first-architecture`, `frontend/frontend-performance`, `frontend/ui-ux-accessibility`, `frontend/frontend-testing`, `frontend/threejs-webgl-development` as relevant. |
| QA Engineer | No universal QA skill currently exists. Use the QA Engineer profile and target repository's test conventions. | `frontend/frontend-testing` for frontend work; `ai/llm-evaluation` for AI behavior; `security/security-review` for security test planning. |
| Security Reviewer | `security/security-review` | `backend/authentication-authorization`, `ai/agent-development`, `ai/tool-calling`, `ai/rag-vector-search`, `architecture/docker-cicd` as relevant. |
| DevOps Engineer | `architecture/docker-cicd` | `ai/ai-observability-cost`, `ai/llm-api-integration`, `backend/database-design` as relevant. |
| Team Lead | No dedicated team-lead skill currently exists. Use the Team Lead profile, workflow, and handoff contract. | `architecture/system-design`, `ai/prompt-engineering`, `ai/llm-evaluation` when the task involves architecture or AI-agent quality. |

## Skill loading rules

1. Read the skill's complete `SKILL.md` before relying on it, when the environment can access it.
2. Start with the role's default skill(s); add conditional skills only when the task scope calls for them.
3. Do not infer a skill is installed or executable merely because its path is listed in this repository.
4. Skills are guidance, not authority to override Product Owner decisions, repository instructions, security policy, or accepted ADRs.
5. Record the skills actually consulted in the task handoff when they materially influenced the work.
6. If two skills conflict, stop and report the conflict; do not silently choose a new project convention.
