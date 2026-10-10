---
name: frontend-design-anti-slop
description: >
  Create, implement, redesign, or audit a frontend UI with a distinctive, product-specific visual direction rather than generic AI-generated design. Covers design briefs, design systems, implementation, responsive behavior, accessibility, and rendered visual verification.
  Trigger: frontend design, UI design, anti-slop, AI slop, generic UI, looks generic, design brief, visual direction, visual identity, redesign UI, landing page design.
license: MIT
metadata:
  author: vekzz-dev
  version: "1.0"
---

# Frontend Design — Anti-Slop

Create frontend interfaces that look intentional, belong to their product, and work reliably. Do not treat visual design as decoration added after implementation. Establish a coherent direction, implement it using the project's actual stack, then inspect the rendered result and correct visible problems.

This skill owns visual direction. When the question is frontend engineering craft (component boundaries, state, routing, testing, performance), defer to the `frontend-engineering` skill if available and keep only design-relevant consequences here.

## Operating principles

1. **Product first, aesthetics second.** Derive design choices from the product, audience, primary tasks, content, brand, and environment. Do not select a fashionable style before understanding what the interface is for.
2. **Specificity over novelty.** A design does not need to be unusual. It must have a defensible visual point of view and avoid looking interchangeable with any unrelated SaaS template.
3. **Use constraints before adding tools.** Inspect the existing app, conventions, tokens, component library, assets, routes, and dependencies. Keep the current stack unless a change is necessary and justified.
4. **Consistency without monotony.** Reuse tokens and components, but do not turn every section into the same card, grid, or repeated hero layout.
5. **Real interface, not a mockup that merely looks clickable.** Implement meaningful navigation, forms, controls, feedback, and loading/empty/error/success states relevant to the task.
6. **Verify honestly.** A successful build is not proof of good visual design. Inspect a rendered page when browser or screenshot tooling is available. Never claim a visual check that was not performed.
7. **No arbitrary bans.** Gradients, cards, pills, shadows, minimalism, and popular typefaces are not automatically bad. Use them only when they serve a clear purpose and fit the design direction.

## Handoff

| Question | Owns it |
|---|---|
| Visual direction: design thesis, design brief, visual system, anti-slop audit, rendered visual verification | This skill |
| Frontend craft: component boundaries, state, routing, API integration, testing, performance | `frontend-engineering` skill, if available |
| Cross-cutting process: requirements discovery, documentation level, artifact organization | `solution-design` skill, if available |
| Figma `.fig` files and editable mockup editing | `open-pencil` skill, if available |
| API/database contract detail | `api-design` / `database-design` skills, if available |

## Workflow

### 1. Reconnaissance: understand the existing project

Before changing code:

- Identify the frontend root, framework, package manager, entry points, routes, existing page/component structure, and current styling approach.
- Read relevant `AGENTS.md`, `README.md`, existing `design.md` or design-system docs, and nearby code. Follow repository-specific instructions.
- Inspect existing assets, fonts, icons, UI libraries, tokens, and components. Reuse them when appropriate; do not duplicate an existing system.
- Determine whether this is a new interface, an extension of an established product, or a redesign. Preserve existing brand and UX decisions unless the request calls for changing them.
- If the frontend is in a separate repository from an API/backend, work only in the frontend repository provided for this task. Do not create or modify files in another repository that is not part of the task.
- Do not install dependencies or replace libraries just to achieve a preferred look. First see what is already available.

### 2. Establish the product and design intent

Extract from the request and repository:

- Product type and purpose.
- Primary user and their most important task.
- Main content and information hierarchy.
- Brand personality or visual references, if provided.
- Functional, technical, accessibility, and responsive constraints.

If information is missing, make reasonable, low-risk assumptions and continue. Record important assumptions briefly in the design brief. Ask a question only when the missing information blocks a consequential decision; do not use questions as a substitute for design judgment.

Write a one-sentence **design thesis**: what the interface should feel like and which visual choices will make that fit its product. Then define three to five principles that can be checked against the implementation. Avoid vague goals such as “modern”, “clean”, “sleek”, or “beautiful” unless translated into concrete decisions.

### 3. Create or update the project design brief

- If an existing design brief or design document exists — for example `docs/frontend/design.md` created by the `frontend-engineering` skill, or any project-level `design.md` — use it as the source of truth and update it only when required by the task. Do not create a competing design system or a second design document.
- If there is no usable design brief and the work involves a substantial new UI or visual redesign, create `design.md` in the frontend project root. For a monorepo, place it in the relevant frontend app directory. Use `templates/design.md` as the starting structure.
- When the `frontend-engineering` skill created or owns `docs/frontend/design.md`, extend its visual sections there instead of creating a separate `design.md`; link the documents instead of duplicating content.
- For a tiny isolated change, do not generate a long document unnecessarily; make a concise note in the task or existing documentation instead.
- Keep `design.md` specific to that product: design thesis, users/tasks, visual principles, typography, color roles, layout, spacing, surfaces, components, motion, imagery, accessibility, and responsive rules.
- Keep reusable agent workflow instructions in this skill. Do not copy the whole skill into `design.md`, and do not create `AGENTS.md` unless asked or the repository's conventions explicitly require it.

### 4. Choose a coherent visual direction before coding

Define concrete choices before building the page:

- **Hierarchy:** what must attract attention first, second, and third; how titles, content, actions, and supporting information differ.
- **Typography:** choose a purposeful type scale, weights, line lengths, and line heights. Prefer fonts already present or an intentional font choice appropriate to the product. Avoid using type size alone to create hierarchy.
- **Color:** define semantic roles (background, surface, text, muted text, border, primary action, focus, success, warning, error). Check contrast. Do not select colors only because they are trendy.
- **Composition:** choose a layout suited to the content—editorial, data-dense, utility-focused, immersive, asymmetric, compact, or otherwise justified. Not every page should be a centered hero plus a grid of cards.
- **Spacing and shape:** establish a small, consistent scale. Use borders, radius, and shadow purposefully; do not put every element in a rounded container.
- **Imagery and iconography:** use relevant, high-quality assets that support meaning. Prefer existing product assets. Do not use random stock art, emoji, or decorative icons as substitutes for missing product content. Icons should be consistent and recognizable.
- **Interaction and motion:** specify hover, active, focus, disabled, loading, and feedback behavior where relevant. Motion should clarify state or hierarchy, not merely decorate. Respect reduced-motion preferences.
- **Responsive behavior:** decide how layout, navigation, density, typography, and controls adapt at narrow widths. Do not treat mobile as desktop content squeezed smaller.

Use the least number of visual motifs needed to give the interface a recognizable identity. A single coherent idea executed well is better than many unrelated effects.

### 5. Implement in the project's conventions

- Build the actual requested interface using the established framework, language, component model, and styling conventions.
- Prefer semantic HTML and accessible native controls. Preserve keyboard operation, visible focus, meaningful labels, heading order, and suitable ARIA only where needed.
- Use design tokens for repeated values instead of scattering near-duplicate colors, radii, shadows, and spacing values.
- Reuse shared components when patterns are truly shared. Do not force different interactions into one abstract component just to maximize reuse.
- Use realistic, context-appropriate content. Do not invent testimonials, customer logos, performance metrics, charts, or status counts that could be mistaken for real data. Label illustrative data clearly when it is necessary.
- Include relevant states: loading, empty, error, success, disabled, validation, and long-content behavior when applicable.
- Keep text readable and layouts robust with long labels, missing optional content, and varying data lengths.
- Do not remove working functionality in pursuit of a screenshot. Visual polish must not break user flows.

### 6. Run a specific anti-slop review

Before finalizing, inspect the page and ask:

- Could this UI be swapped into an unrelated product by changing only its logo and title? If yes, increase product-specific structure, content hierarchy, terminology, or visual identity.
- Did the layout come from the content and task, or from a default template?
- Is there a clear visual hierarchy, or does everything compete for attention?
- Are repeated cards, pills, borders, shadows, gradients, and icons doing useful work?
- Does typography have a deliberate role and scale?
- Is there a memorable but restrained visual idea, or merely a pile of trendy effects?
- Is the page too empty, too dense, or padded to imitate a generic marketing template?
- Are all controls functional and all visible data credible?
- Does the responsive layout have deliberate decisions at narrow widths?

Common warning signs to challenge (not automatically forbid):

- A purple/blue gradient on a dark background with glow effects, chosen without product rationale.
- The same large headline, subheading, two buttons, and floating mockup used for every landing page.
- A bento grid or card grid used regardless of the content's natural relationships.
- Wrapping every section, metric, button, or label in a rounded pill/card.
- Decorative blobs, glassmorphism, excessive blur, random gradients, and shadows that do not convey hierarchy or state.
- Generic copy such as repeated “Empower your workflow” phrases, fake metrics, lorem ipsum, or implausible content.
- Too many font sizes, accent colors, radii, icon styles, or competing visual metaphors.
- Stock photography unrelated to the product or icons used only to fill empty space.
- Hover effects without feedback, animation that delays tasks, or clickable-looking elements that do nothing.

Do not “fix” generic design by adding arbitrary decoration. Prefer adjusting the information architecture, proportions, density, typography, content, imagery, and layout.

### 7. Verify the rendered interface

Use available tools in this order of preference: run the app, open it in a browser, inspect representative states, capture screenshots, and iterate. If browser automation is already available, use it; do not add a new dependency solely for a simple screenshot unless necessary.

At minimum, when feasible, review:

- A desktop viewport around 1440 × 900.
- A narrow mobile viewport around 390 × 844.
- The primary task/flow and one important alternate state (for example empty, validation error, or open navigation).

Check for overflow, clipped text, uneven alignment, awkward line breaks, weak contrast, inconsistent spacing, oversized whitespace, cramped mobile controls, and layout changes that damage hierarchy. Correct the visible issues and inspect again.

Read `references/visual-audit-checklist.md` for the complete audit. If the application cannot be run or browser/screenshot tooling is unavailable, execute the feasible checks and explicitly state that visual verification could not be completed. Do not imply that tests or screenshots exist if they do not.

### 8. Completion criteria

Do not finish until the feasible criteria below pass:

- [ ] Design decisions fit the product, content, and users rather than a generic template.
- [ ] An intentional visual direction exists and is reflected consistently in the interface.
- [ ] Existing project conventions, branding, and working functionality are respected.
- [ ] Layout and type hierarchy are clear at desktop and narrow widths.
- [ ] Controls and relevant states work; no fake interactions or invented real-looking data were added.
- [ ] Accessibility basics (semantic structure, keyboard access, focus, labels, contrast) were considered.
- [ ] Build, lint, type checks, or tests relevant to the changes were run when available.
- [ ] Rendered output was inspected when tools allowed, and visible defects were corrected.
- [ ] The final response states what changed, which checks ran, and any material verification limitation.

If a criterion cannot be verified, say so explicitly. Never fabricate a successful build, test run, browser check, or design result.
