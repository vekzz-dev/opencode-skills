---
name: frontend-engineering
description: "Trigger: frontend engineering, frontend architecture, UI design, component design, state management, accessibility, design system, UI review, frontend docs. Guide frontend work from discovery to delivery, adapting rigor to size and risk."
license: MIT
metadata:
  author: "vekzz-dev"
  version: "1.1.0"
  domain: "frontend engineering"
---

# Frontend Engineering

Help plan, design, build, and maintain frontend software with an appropriate level of rigor. Apply the skill to web frontends and, where relevant, mobile or desktop clients. Framework-agnostic: never assume a particular framework, design system, state library, or build tool; adapt examples to the actual stack.

This skill owns frontend craft. When the question is cross-cutting process (documentation level, artifact organization, requirement discovery), defer to the `solution-design` skill if available and keep only frontend-relevant consequences here.

## Activation Contract

Load this skill when the user:

- starts a frontend project from scratch;
- plans a UI, user flow, component system, or frontend architecture;
- implements a feature spanning multiple screens or components;
- reviews frontend maintainability, UX, accessibility, performance, or testing;
- creates or updates frontend design documentation;
- aligns frontend behavior with a backend API or product requirements.

Do not load for a tiny, isolated change: inspect the relevant context, implement it, run appropriate checks, and summarize remaining risks — no full workflow.

## Hard Rules

1. **Understand before changing code.** For an existing project, inspect the repository, package manifests, routing, components, styling, tests, and existing documentation before proposing changes. Never assume a clean slate.
2. **Adapt to scale and risk.** Choose lightweight, standard, or rigorous using complexity, user impact, accessibility needs, security/privacy, performance requirements, integrations, team size, and uncertainty — not only the number of screens.
3. **Avoid document inflation.** Capture important decisions; create a separate file only when separation improves collaboration, review, traceability, or maintenance.
4. **Separate requirements from implementation.** State what users need apart from the technical approach used to deliver it.
5. **Make assumptions visible.** Distinguish confirmed requirements, assumptions, open questions, and recommendations. Never silently invent product behavior, roles, branding, or APIs.
6. **Design states, not just happy paths.** Consider loading, empty, error, success, disabled, permission-denied, offline, and partial-data states when relevant.
7. **Accessibility and responsive behavior are quality requirements** from the beginning, not a final polish pass.
8. **Keep documentation aligned with the code.** Update only affected documentation; one source of truth, link instead of duplicating.
9. **Validate incrementally.** Prefer small, testable slices of user-visible functionality over a large speculative design phase.
10. **Follow the project's conventions.** Do not introduce new libraries or architectural patterns without a reason and an explicit trade-off.
11. **The client is not a trust boundary.** Never put secrets or privileged authorization logic in client code; client-side validation improves usability but never replaces server-side validation.

## Decision Gates

### Documentation level

| Signals observed | Level | Artifacts |
|---|---|---|
| Small scope, low risk, few views/flows, one developer or rapid prototype | **Lightweight** | One `docs/frontend/design.md` (see `templates/frontend-design.md`) plus README if needed; combine requirements, flows, architecture, UI rules, API assumptions, accessibility, tests |
| Several features, shared components, meaningful client state, integrations, or a team | **Standard** | Concise design overview; add focused documents (`ui-flows.md`, `design-system.md`, `frontend-architecture.md`, `frontend-testing.md`, ADRs) only when separate ownership or frequent updates justify them |
| High-impact or regulated journeys, sensitive data, complex permissions, strict accessibility/performance targets, many teams, large application | **Rigorous** | Requirements traceability, design-system documentation, architecture decision records, test strategy, performance budgets, security/privacy reviews as applicable |

A project can be small but high-risk: increase rigor where risk warrants it. Reassess the level when scope changes. Reassessment triggers and consolidation criteria: [`references/workflow.md`](references/workflow.md).

### Handoff

| Question | Owns it |
|---|---|
| Cross-cutting process: requirements discovery, documentation level, artifact organization | `solution-design` skill, if available; otherwise keep the lightweight process above |
| Frontend craft: component boundaries, state, UX, accessibility, testing, performance | This skill |
| API/database contract detail | `api-design` / `database-design` skills, if available |

## Execution Steps

1. **Establish context.** Greenfield: clarify product goal, users, primary tasks, devices/browsers, brand constraints, accessibility expectations, data sensitivity, API dependencies, and constraints; ask focused questions only when they materially affect the design, otherwise record assumptions. Existing project: inspect framework versions, routes, styling strategy, component conventions, state/data fetching, API contracts, tests, and the current design system first.
2. **Select the documentation level** from the Decision Gate above and briefly state why.
3. **Define user and interface requirements.** User types and goals, key journeys, navigation, screen responsibilities, responsive behavior, and acceptance criteria. If access differs by role, document which actions and views are allowed, hidden, disabled, or rejected — visibility is a UX rule, not the security boundary. Wireframes/diagrams only when they reduce ambiguity; if visual design is requested, establish the output format (image, editable mockup, or implementation) before choosing.
4. **Design frontend architecture.** Routes, component responsibilities (shared vs feature-specific), state ownership and data flow, API client behavior, forms and validation, permission-aware presentation, configuration, localization and error boundaries if appropriate. Prefer the simplest architecture that meets current requirements; no speculative frameworks, unnecessary global state, or premature abstraction. Reason with [`references/frontend-architecture.md`](references/frontend-architecture.md).
5. **Define UX and visual behavior.** Information hierarchy, interaction patterns, design tokens and component variants when a design system is justified, form UX, all interface states, responsive layouts, and the accessibility baseline. Extend an existing design system instead of creating a competing one; never invent brand colors or typography — place an explicit placeholder or ask. Reason with [`references/ui-ux-accessibility.md`](references/ui-ux-accessibility.md).
6. **Align API and data contracts.** Confirm endpoints, methods, schemas, pagination, validation errors, and authentication expectations against the backend contract; reuse existing OpenAPI or generated clients. If the contract is unknown, label it as an assumption and never claim integration. Details: [`references/frontend-architecture.md`](references/frontend-architecture.md).
7. **Plan quality from the beginning.** Select checks proportionate to the project — unit/component/integration/e2e tests, accessibility (automated plus keyboard/manual), responsive/cross-browser per support targets, performance (bundle, rendering, Core Web Vitals) when relevant, visual regression only when justified. Tie acceptance criteria to observable tests or a documented manual method. Plan with [`references/quality-testing.md`](references/quality-testing.md).
8. **Plan implementation in vertical slices.** Small increments delivering a complete user-visible capability; for each, identify screens/components, API dependencies, states, acceptance criteria, and tests. Resolve high-risk unknowns early; do not require full specification of every screen before a useful first increment.
9. **Implement, verify, and maintain.** Before editing, summarize the intended scope for significant work. Follow repository conventions, run relevant checks, and report checks not run and why. Update documentation when a material decision, interaction, contract, or architecture changes. For review: correctness, accessibility, security/privacy, responsive behavior, maintainability, performance, regression risk; distinguish verified issues from potential concerns.

## Documentation Rules

- Start with the smallest useful set of documents; explain briefly why any separate artifact is needed.
- Lightweight: use [`templates/frontend-design.md`](templates/frontend-design.md) as the basis for a single `docs/frontend/design.md`.
- Standard/rigorous: split sections only when the split improves use or maintenance. Do not duplicate complete API definitions, product requirements, or backend/database specifications — link to their canonical documents and record only frontend-relevant consequences.
- Mark sections `Not applicable` only when the omission needs explanation; otherwise omit them.
- Label decisions `Confirmed`, `Proposed`, or `Open question` when the status matters.

## Reference Guide

Read only the references relevant to the current task:

- [`references/workflow.md`](references/workflow.md): project lifecycle, reassessment triggers, consolidation criteria.
- [`references/frontend-architecture.md`](references/frontend-architecture.md): architecture, component boundaries, state, routing, API integration.
- [`references/ui-ux-accessibility.md`](references/ui-ux-accessibility.md): user flows, interface states, responsive design, accessibility, design systems.
- [`references/quality-testing.md`](references/quality-testing.md): testing, performance, security/privacy, release checks.
- [`references/document-templates.md`](references/document-templates.md): suggested sections for lightweight and specialized documents.
- [`templates/frontend-design.md`](templates/frontend-design.md): concise all-in-one design document template for small projects.

## Output Contract

At the end of a design or implementation task, report:

1. Files created or modified (including where each documented responsibility lives if artifacts were combined).
2. The documentation level chosen and why.
3. The most important assumptions.
4. Open questions, blockers, and pending high-impact decisions.
5. Checks that were run, checks that were not run and why.
6. The next increment recommended.

## Completion Checklist

Before considering a design or implementation task complete, verify the relevant items:

- user goals and scope are understood;
- key flows and acceptance criteria are clear;
- component and state responsibilities are coherent;
- API dependencies and unknowns are explicit;
- loading, empty, error, success, and permission states are considered where relevant;
- responsive behavior and accessibility are addressed;
- security-sensitive behavior is not entrusted to the client alone;
- appropriate tests/checks are run, or remaining gaps are reported;
- documentation is concise, current, and not unnecessarily duplicated.
