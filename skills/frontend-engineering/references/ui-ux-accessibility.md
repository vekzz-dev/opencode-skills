# UI/UX, Responsive Design, Accessibility, and Design Systems

## User journeys and screen inventory

For each important journey, capture:
- user goal and starting point;
- primary steps and decisions;
- success outcome;
- alternate paths and failure/recovery behavior;
- permissions or role differences, if any;
- acceptance criteria.

A screen inventory may include route/view, purpose, primary action, data dependencies, and key UI states. Use diagrams or wireframes when they reduce ambiguity; do not require them for trivial changes.

## Interface states

For each data-driven or interactive view, consider applicable states:
- initial/loading and background refresh;
- populated/success;
- empty/no results;
- validation error;
- server/network error and retry;
- disabled/in-progress;
- unauthorized/forbidden;
- stale, partial, or offline data where relevant.

Give users a clear explanation and a recovery action when possible. Avoid indefinite spinners and errors that provide no next step.

## Visual design and design tokens

When a design system is needed, document tokens for color, typography, spacing, sizing, borders, elevation, breakpoints, and motion. Explain semantic intent (for example, danger, success, surface, focus) rather than relying only on raw values. Include component variants and usage rules only for components that need consistency.

Use existing brand assets and design tokens when present. Do not fabricate a brand identity from assumptions. If visual direction is missing, ask targeted questions or mark a proposed direction clearly.

## Responsive design

Define supported viewport/device ranges based on product needs, not arbitrary device models. Specify how layouts reflow, navigation changes, tables/data-heavy content adapt, touch targets behave, and dialogs/menus work at narrow widths. Avoid relying only on fixed pixel dimensions.

## Accessibility baseline

Aim for WCAG 2.2 AA where applicable and agree on the target for the project. At minimum consider:
- semantic HTML and correct heading hierarchy;
- accessible names and labels for controls;
- keyboard operation and visible focus;
- logical focus order and focus management in dialogs/navigation;
- sufficient text and non-text contrast;
- error identification and instructions;
- status announcements for asynchronous updates where needed;
- zoom/reflow and reduced-motion preferences;
- alternatives to color-only communication;
- captions/transcripts for relevant media.

Automated accessibility tools are useful but cannot prove accessibility. Verify critical journeys with keyboard interaction and manual review; include assistive technology checks when project risk warrants them.

## Content and interaction design

Use consistent labels, clear action names, helpful validation, confirmation for destructive or irreversible actions, and recovery paths. Keep error messages actionable and avoid exposing sensitive technical details. Consider localization, date/time formats, pluralization, and long translated strings when relevant.
