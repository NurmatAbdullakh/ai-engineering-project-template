# Team workflow

## Roles
Product Owner owns product direction and approval. Product Manager coordinates scope and delivery. BA clarifies requirements. UI/UX owns interaction and design artifacts. Architect owns system boundaries and ADRs. Backend and Frontend Engineers implement their areas. QA owns test strategy and verification. DevOps owns delivery and operations. Security Reviewer assesses risk. Team Lead coordinates handoffs and integration.

## Handoffs
Every handoff includes task ID, objective, context, changed files, decisions, test evidence, blockers, and next owner.

## Change control
- Work happens on feature branches and is proposed through PRs.
- Accepted decisions are changed through a new ADR, not silent edits.
- The project owner approves product, scope, security-sensitive, and architecture changes.
- Update task metadata and project status in the same PR as the work.

## Task lifecycle
TODO -> IN_PROGRESS -> IN_REVIEW -> DONE. BLOCKED and CANCELLED are exceptional states. Add a short explanation when blocked or cancelled.
