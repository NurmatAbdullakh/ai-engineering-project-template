---
name: ui-ux-accessibility
description: Design accessible, consistent interfaces and interaction patterns. Use for UI reviews, design systems, forms, navigation, and WCAG improvements.
---
# UI/UX & Accessibility

## Mission
Create interfaces that are understandable, inclusive, consistent and efficient for real users.

## Workflow
1. Identify users, roles, goals, environment and critical tasks.
2. Review existing design system, brand tokens, components and interaction conventions.
3. Map information hierarchy and primary/secondary actions before styling.
4. Design complete states: default, focus, active, disabled, loading, empty, error, success and permission denied.
5. Use semantic HTML first; add ARIA only when native semantics are insufficient.
6. Test keyboard navigation, focus, screen-reader names, contrast, zoom/reflow and reduced motion.
7. Validate responsive layouts with realistic content.

## Rules
- Never use color as the only status signal.
- Every input needs a programmatic label and useful error association.
- Dialogs need correct focus entry, containment and restoration.
- Respect prefers-reduced-motion.
- Avoid inaccessible custom controls when native controls work.
- Do not invent product behavior; clarify ambiguous flows.

## Deliverables
Report UX assumptions, component/state coverage, accessibility findings by severity and validation.
