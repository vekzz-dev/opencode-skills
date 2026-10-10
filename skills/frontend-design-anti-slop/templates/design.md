# Design Brief — [Product / App Name]

> Product-specific visual and interaction decisions. Keep this document grounded in the actual application; replace every bracketed prompt and delete sections that do not apply.

## 1. Product context

- **Product purpose:** [What does this application help people do?]
- **Primary users:** [Who uses it?]
- **Primary task(s):** [What must users be able to accomplish quickly?]
- **Most important screens:** [Routes/screens in scope]
- **Brand / product constraints:** [Existing logo, colors, content, assets, legal or technical constraints]
- **Assumptions:** [Only important assumptions made because information was unavailable]

## 2. Design thesis

[One sentence describing the intended character of this interface and why it fits this product. Avoid ungrounded adjectives such as “modern” or “clean”.]

### Design principles

1. **[Principle]:** [Specific behavior or visual decision that expresses it.]
2. **[Principle]:** [Specific behavior or visual decision that expresses it.]
3. **[Principle]:** [Specific behavior or visual decision that expresses it.]

### What this design is not

- [Generic pattern or visual approach to avoid for this product, with reason]
- [Another mismatch to avoid, with reason]

## 3. Visual system

### Typography

- **Primary typeface:** [Name and reason]
- **Secondary / monospace typeface:** [If needed]
- **Type scale:** [Body, small text, labels, headings, display sizes]
- **Rules:** [Line length, line height, weight, capitalization, number formatting]

### Color roles

| Token / role | Value | Purpose / usage |
|---|---|---|
| `color-bg` | [value] | Main page background |
| `color-surface` | [value] | Elevated or grouped content |
| `color-text` | [value] | Primary text |
| `color-text-muted` | [value] | Secondary text; maintain sufficient contrast |
| `color-border` | [value] | Dividers and control outlines |
| `color-primary` | [value] | Primary actions and key emphasis |
| `color-focus` | [value] | Keyboard focus indicator |
| `color-success` | [value] | Positive status |
| `color-warning` | [value] | Warning status |
| `color-danger` | [value] | Errors and destructive actions |

### Layout and spacing

- **Content width / grid:** [Max width, columns, gutters, alignment]
- **Spacing scale:** [Small, medium, large, section spacing]
- **Page rhythm:** [How sections and content groups are ordered and spaced]
- **Density:** [Compact / balanced / spacious, and why this suits the task]
- **Responsive changes:** [What rearranges, collapses, moves, or disappears at narrow widths]

### Surfaces and shapes

- **Border radius:** [Allowed sizes and where used]
- **Borders and shadows:** [What elevation means in this interface]
- **Background treatments:** [Where solid color, texture, gradient, or imagery is justified]
- **Containers:** [When to use a card, divider, list, table, plain section, or other grouping]

### Imagery and icons

- **Image style / source:** [Product assets, photography, illustration, charts, or no imagery]
- **Icon set:** [Existing icon library or convention]
- **Rules:** [Sizing, stroke weight, labeling, image crop, fallback behavior]

## 4. Components and interaction patterns

| Pattern / component | When to use | Behavior and states |
|---|---|---|
| Primary action | [Use case] | [Default, hover, focus, disabled, loading] |
| Secondary action | [Use case] | [Behavior] |
| Forms | [Use case] | [Labels, validation, errors, success] |
| Navigation | [Use case] | [Active state, mobile behavior, keyboard support] |
| Data display | [Use case] | [Sorting, empty state, loading, errors, long content] |
| Feedback | [Use case] | [Toast, inline message, confirmation, status] |

Add only patterns relevant to the product. Do not create a component catalogue for its own sake.

## 5. Motion and feedback

- **Motion principle:** [No motion / restrained / expressive and why]
- **Allowed transitions:** [Duration and purpose]
- **Reduced motion:** [How `prefers-reduced-motion` is respected]
- **Feedback rules:** [How actions, errors, loading, and completion are communicated]

## 6. Accessibility and responsive behavior

- **Keyboard and focus:** [Navigation and visible focus behavior]
- **Semantics and labels:** [Heading order, landmark structure, form labels, accessible names]
- **Contrast:** [How text and controls will be checked]
- **Screen readers:** [Necessary announcements and status semantics]
- **Mobile:** [Touch target, navigation, content order, scrolling, and overflow rules]
- **Zoom / long content:** [Behavior under zoom and with long names or translated text]

## 7. Page-specific composition

### [Route / screen name]

- **User goal:** [What the user needs to accomplish]
- **Primary hierarchy:** [First, second, third focus]
- **Layout:** [Chosen composition and why it suits the content]
- **Primary action:** [Action]
- **States to support:** [Loading, empty, error, validation, success, etc.]

[Repeat for screens in scope.]

## 8. Anti-slop acceptance checks

- [ ] The design reflects this product's actual purpose, users, and content.
- [ ] A coherent visual idea is visible without relying on generic decoration.
- [ ] Layout is chosen for the information and task, not defaulted to repeated cards.
- [ ] Typography, color, spacing, shapes, and icons follow explicit roles.
- [ ] No fabricated social proof, metrics, customer identities, or real-looking sample data.
- [ ] Mobile has deliberate composition rather than simply shrinking desktop.
- [ ] Focus, contrast, labels, keyboard behavior, and important UI states are covered.
- [ ] Rendered desktop and mobile views have been inspected where tooling allows.

## 9. Decisions and open questions

- **Decision:** [Choice] — **Reason:** [Product-specific rationale]
- **Open question:** [Question that materially affects design; omit if none]
