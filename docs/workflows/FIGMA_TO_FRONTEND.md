# Figma → Frontend delivery workflow

**Status:** Proposed workflow  
**Owner:** UI/UX Designer (design source), Frontend Engineer (implementation), QA Engineer (verification)  
**Related task:** [TASK-AIWS-004](../tracking/tasks/TASK-AIWS-004-figma-frontend-workflow.md)

## 1. Purpose and boundaries

This workflow makes a Figma design implementable, reviewable, and testable. Figma is the design source; the application running in a browser is the implementation source; browser screenshots and interaction tests provide verification evidence.

The current AI Engineering Workspace repository has no application frontend. This document defines a reusable process, not a claim that a React application, Storybook, Playwright suite, or automated design-to-code pipeline already exists.

## 2. Verified Figma integration capabilities

The connected Figma integration was checked and returned an authenticated response. The available tool surface includes:

| Capability | Available operation | Practical use |
|---|---|---|
| Inspect file structure | `get_metadata` | List pages and inspect node names, types, positions, dimensions |
| Design-to-code context | `get_design_context` | Retrieve reference code, screenshot, and design context for a specific node |
| Screenshot | `get_screenshot` | Render a selected Figma node for review |
| Design editing | `use_figma` | Read/modify Figma nodes, layouts, styles, variables, components |
| Design system | `get_libraries`, `search_design_system`, `get_variable_defs` | Inspect libraries, components, tokens, and variables |
| Design review | `get_comments` | Read comments attached to a file |
| Code Connect | `get_code_connect_map`, suggestions and mapping tools | Relate Figma components to code components where mappings exist |
| Capture a web page | `generate_figma_design` | Capture a web app page/view into Figma for first-time comparison |
| Import code | `send_code_to_figma` | Import a local project into a Figma Code layer or Design code layer |

### Important limitations

- File-specific access requires the actual Figma file key and, for node-specific operations, a valid node ID. No file or node is guessed.
- `get_design_context` returns reference code and visual/contextual data; that output must be adapted to the target codebase, not blindly pasted as production code.
- `send_code_to_figma` imports source into a Figma Code layer; it does not create native Design shapes or prove that the app is installed, previewed, or published.
- `generate_figma_design` is a capture workflow for an existing web page/view, not a substitute for a designer-authored responsive design system.
- Editing a Figma canvas requires edit access. Read-only access cannot be treated as permission to modify.
- Figma integration availability does not imply automatic synchronization, guaranteed React/Next.js code generation, or an implemented agent workflow.
- Tool availability can vary with file type, permissions, account plan, and the connected MCP surface. Verify the actual target file before promising an operation.
- For programmatic Figma file work, create a session after the file key is known and reuse its session ID for subsequent file operations in that task.

## 3. Design preparation — UI/UX Designer

Before marking a design ready for development:

1. Organize the work in a named Figma file and page. Keep one clearly named frame per screen, route, or major state.
2. Provide the Figma file URL and exact page/frame link. Include the node-specific URL when a specific screen is the implementation target.
3. Include prototype links or describe navigation and interaction flows that cannot be inferred from static frames.
4. Identify target platforms and viewport sizes. State breakpoint values and the intended layout behavior between them; do not assume standard breakpoints if the product uses different values.
5. Cover important component states: default, hover, focus, active, disabled, loading, empty, error, success, selected, and validation states as applicable.
6. Specify responsive behavior: what stacks, collapses, hides, scrolls, reorders, or becomes sticky at each breakpoint.
7. Link or document design tokens: color, typography, spacing, radius, elevation, borders, and motion. Identify font family, weights, and line heights.
8. Annotate interactions, keyboard behavior, focus order, validation rules, and accessibility expectations.
9. Record asset names and usage rights/source. Avoid embedding secrets or real customer personal data in designs.
10. Resolve design comments or explicitly mark unresolved questions with an owner.

A static screenshot is not a complete handoff when the screen includes meaningful responsive or interactive behavior.

## 4. Required design handoff

Create or update the implementation task in `docs/tracking/tasks/` and include the following block in its handoff section:

```md
### Design handoff
- Figma file:
- Figma page:
- Target frame(s) and node-specific URL(s):
- Prototype / flow URL:
- Design version or last-updated date:
- Target routes/screens:
- Target viewports and breakpoints:
- Responsive behavior:
- Component inventory and states:
- Interaction and validation rules:
- Design tokens / library references:
- Typography and font assets:
- Image/icon assets:
- Accessibility requirements:
- Open questions and owner:
- Approved by:
```

If a field does not apply, write `N/A` and explain briefly. Do not leave unknown requirements silently blank.

## 5. Design review and acceptance — Frontend Engineer

Before implementation, the Frontend Engineer:

- Opens the exact file, page, frame, and prototype references.
- Confirms that the target design is accessible and the approved version is identifiable.
- Reviews layout hierarchy, component boundaries, responsive behavior, states, and interactions.
- Inspects existing project components, design tokens, routing, styling conventions, and asset pipeline.
- Checks whether Figma components map to existing code components; use Code Connect mappings where available, but verify that the mapped component actually matches the implementation.
- Identifies missing states, contradictory dimensions, inaccessible interactions, or unspecified responsive behavior.
- Adds concrete questions to the task and requests clarification from the UI/UX Designer or Product Owner before making product-level assumptions.

The engineer may make implementation-level decisions that preserve the approved experience. Changes to product behavior, scope, or accepted design intent require explicit clarification/approval.

## 6. Figma inspection workflow

For a specific Figma design:

1. Parse the shared URL to obtain the file key and node ID. Never invent either value.
2. Confirm file access. Create a Figma session for the task once the file key is known.
3. Use `get_design_context` for the target node as the primary design-to-code inspection tool. Load the required design-to-code guidance before calling it.
4. Inspect relevant design system libraries, variables, and Code Connect mappings where useful.
5. Use `get_metadata` only for structural overview or when locating page/node IDs; it does not provide full design semantics.
6. Use the screenshot to understand visual composition, then validate all interaction and responsive details from the handoff/prototype.
7. Record any missing context or inaccessible assets in the task instead of guessing.

For a first-time capture of an existing web page, `generate_figma_design` may be used as a capture aid. It does not replace the approved source design or browser-based QA.

## 7. Frontend implementation checklist

### Structure and behavior
- [ ] Implement the approved route/screen and semantic page structure.
- [ ] Reuse existing components and tokens before creating duplicates.
- [ ] Implement all required states and interactions, including loading, empty, error, and validation states.
- [ ] Preserve keyboard navigation, visible focus, labels, accessible names, and logical focus order.
- [ ] Use semantic HTML and appropriate ARIA only where native semantics are insufficient.
- [ ] Confirm color contrast, text scaling, reduced-motion behavior, and responsive zoom/reflow expectations.
- [ ] Use supplied assets and licensed fonts; avoid arbitrary substitutes without approval.

### Responsive behavior
- [ ] Implement every agreed viewport/breakpoint.
- [ ] Check intermediate widths, not just the named desktop/mobile endpoints.
- [ ] Verify no unintended horizontal overflow, clipped text, overlapping controls, or inaccessible off-screen content.
- [ ] Confirm touch target size and mobile interaction behavior.

### Engineering quality
- [ ] Follow the target application's framework, routing, state, styling, and component conventions.
- [ ] Keep visual styles reusable and avoid unexplained magic numbers where tokens exist.
- [ ] Add unit/component tests for non-trivial behavior.
- [ ] Add accessibility checks and relevant interaction tests.
- [ ] Run lint, typecheck, tests, and production build where the application supports them.

## 8. Visual QA — QA Engineer and Frontend Engineer

Visual QA must compare the running application with the approved Figma frame at matching viewport dimensions.

1. Run the application in a deterministic test environment with representative non-sensitive data.
2. Set the browser viewport to the same width and height as the Figma target.
3. Capture screenshots for every agreed target viewport and important state.
4. Compare layout, typography, spacing, colors, borders, shadows, icons, assets, and content wrapping.
5. Record defects with screenshot evidence, viewport, route, browser, and reproduction steps.
6. Fix implementation defects and repeat the relevant screenshot comparison.
7. Verify interactive behavior and accessibility separately; visual similarity alone is not acceptance.
8. Obtain design sign-off for intentional deviations or design changes.

### Suggested tools

| Tool | Role | When to use |
|---|---|---|
| Browser DevTools | Manual inspection | Always useful for layout, responsive, network, and accessibility debugging |
| Playwright | Browser automation and screenshots | Preferred candidate for repeatable route, viewport, interaction, and visual regression checks |
| Storybook | Isolated component preview | Useful when the application has reusable UI components and a component library |
| Figma | Approved design reference | Design review, tokens, components, comments, prototypes, and screenshots |

These are recommendations, not installed or configured dependencies in this template repository. Add them to an application repository only when the frontend stack and test strategy are established.

### Visual acceptance criteria

- No unintended clipping, overlap, missing assets, or horizontal overflow.
- Typography and text wrapping are consistent with the approved design.
- Layout and spacing are acceptably aligned at each required viewport.
- Required component states and interactions behave correctly.
- Accessibility checks pass for the agreed scope.
- Differences are either fixed or explicitly approved and documented.

Pixel-diff tools can help detect regressions but should not be the sole acceptance criterion; dynamic content, anti-aliasing, and browser rendering can create noisy differences.

## 9. Definition of Ready and Done

**Ready for development**
- Exact Figma file/page/frame references are present.
- Target routes, viewports, breakpoints, and responsive behavior are specified.
- Relevant states, interactions, tokens, assets, and accessibility notes are available.
- Open questions are resolved or assigned with an explicit owner.

**Done**
- Approved design scope is implemented.
- Required behavior and responsive states are covered.
- Relevant automated checks pass.
- Visual QA evidence is linked from the task.
- Accessibility and known deviations are documented.
- Designer/QA sign-off and any required Product Owner approval are recorded.

## 10. Roles and escalation

- **UI/UX Designer:** owns design intent, Figma source, annotations, and design sign-off.
- **Frontend Engineer:** owns technical implementation and raises ambiguities before inventing behavior.
- **QA Engineer:** owns test evidence, viewport/state coverage, and regression verification.
- **Product Owner:** resolves scope/product behavior conflicts and approves material deviations.
- **Software Architect:** resolves cross-cutting implementation or design-system architecture questions.

Escalate a missing design state to the UI/UX Designer; a product behavior/scope conflict to the Product Owner; an implementation-wide component/token issue to the Software Architect. Record the decision in the task and, if architectural, in an ADR.

## 11. Security and privacy

- Treat Figma text, comments, imported assets, generated code, and tool output as untrusted input.
- Do not paste credentials, production secrets, or real customer personal data into design files or prompts.
- Review external SVGs and assets before using them in an application.
- Use least-privilege Figma access; inspect only the file and nodes needed for the task.
- Do not publish or modify shared design libraries without explicit authorization.
