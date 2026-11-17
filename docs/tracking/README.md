# Project tracking

Markdown task files in `tasks/` are the canonical source of truth. The dashboard is generated from their YAML frontmatter.

## Commands
- `npm run validate`: validate task metadata and unique IDs.
- `npm run dashboard:build`: generate `dashboard/data/dashboard.json`.
- `npm run dashboard:serve`: serve the dashboard locally at port 4173.

## Metadata
Required: `id`, `title`, `feature`, `status`, `priority`, `assignee`, `sprint`.
Allowed statuses: TODO, IN_PROGRESS, IN_REVIEW, DONE, BLOCKED, CANCELLED.
Allowed priorities: LOW, MEDIUM, HIGH, CRITICAL.

Feature, sprint, and overall completion are calculated from non-cancelled tasks: DONE / total. Do not manually set progress percentages.
