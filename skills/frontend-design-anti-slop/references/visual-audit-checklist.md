# Visual Audit Checklist

Use this checklist after implementation and after substantial redesigns. Review the rendered interface, not only source code. Mark an item as pass, needs work, or not applicable; record concrete evidence for anything that needs work.

## 1. Product specificity

- Can the interface be transferred to an unrelated product by changing only the name and logo? If yes, look for opportunities to make structure, content, terminology, information density, or visual choices more specific.
- Does the layout support the main user task and likely frequency of use?
- Is the content realistic, coherent, and appropriately prioritized?
- Are all numbers, testimonials, logos, badges, and charts either genuine project data or clearly illustrative?

## 2. Hierarchy and composition

- Is the primary task or information immediately identifiable?
- Are heading, body, metadata, labels, and actions visually distinct for meaningful reasons?
- Is the layout matched to the content (for example, table/list for comparison, form for data entry, editorial composition for narrative content)?
- Do sections have a deliberate rhythm, or are they all repeated card grids?
- Are alignment, widths, gutters, line lengths, and whitespace intentional?
- Is any element decorative but distracting from the task?

## 3. Typography

- Is there a controlled and purposeful type scale?
- Are font families and weights consistent and appropriate?
- Are headings, body text, labels, and numbers readable at all relevant widths?
- Are line breaks, text measure, line height, and truncation appropriate?
- Are generic headings/copy hiding a lack of product-specific content?

## 4. Color and surfaces

- Do colors have stable semantic roles?
- Is primary color emphasis reserved for important actions or information?
- Are text, controls, borders, and status indicators legible against their backgrounds?
- Are gradients, blur, glass effects, shadows, and glow used for a specific reason?
- Does every card/container clarify grouping or hierarchy? Remove unnecessary containers.
- Are radius, border, and shadow styles consistent rather than randomly mixed?

## 5. Components and content states

- Do buttons, links, tabs, menus, form fields, filters, and pagination behave as their appearance implies?
- Are loading, empty, error, success, disabled, and validation states present where relevant?
- Do forms explain how to correct invalid input?
- Do long names, unusually large values, no results, or missing optional data break the layout?
- Are icon meanings recognizable and icon styles consistent?
- Is the interface free of filler icons, decorative chips, fake metrics, and false social proof?

## 6. Responsive behavior

Inspect at least a desktop and narrow mobile viewport when feasible.

- Is there horizontal overflow, clipped text, overlap, or broken alignment?
- Does navigation remain usable?
- Do columns and data displays adapt sensibly (reflow, scroll, collapse, or transform as appropriate)?
- Are touch targets comfortable and actions reachable?
- Does text remain readable without awkward wrapping or excessive truncation?
- Does the mobile layout retain the correct priority and order rather than becoming a compressed desktop grid?

## 7. Accessibility and interaction

- Can all interactive controls be reached and operated by keyboard?
- Is focus visible and not obscured?
- Do controls have accessible names and form fields have labels?
- Is semantic heading and landmark structure appropriate?
- Is color not the only way to communicate status?
- Are motion and transitions respectful of reduced-motion settings?
- Are dialogs, notifications, validation, and dynamic updates communicated appropriately?

## 8. Rendered-output review

When tools permit:

1. Start the app using the repository's existing instructions.
2. Navigate to the changed screen and exercise the primary flow.
3. Capture a desktop view around 1440 × 900 and a narrow view around 390 × 844.
4. Inspect at least one important alternate state.
5. Identify the three most visible defects, fix them, and capture/review again.
6. Run the relevant build, lint, type checks, and tests.

If rendering cannot be inspected, say so in the completion report and state which substitute checks were performed. Do not present a code-only review as a rendered visual audit.

## 9. Triage

Fix in this order:

1. Broken functionality, inaccessible controls, and serious overflow.
2. Incorrect information hierarchy or task flow.
3. Responsive layout, readability, and contrast problems.
4. Inconsistent typography, spacing, tokens, or component behavior.
5. Unnecessary decoration and minor visual polish.

Do not spend time tuning shadows while the main task, layout, or mobile view is broken.

## Audit report template

- **Scope / route:**
- **Viewports and states inspected:**
- **Build / tests run:**
- **Passes:**
- **Issues found:** [Issue, severity, evidence]
- **Corrections made:**
- **Unverified items / limitations:**
