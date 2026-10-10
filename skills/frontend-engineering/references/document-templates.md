# Frontend Documentation Templates

Use the smallest set that serves the project. These are suggested sections, not mandatory paperwork.

## All-in-one frontend design (small projects)

Use `templates/frontend-design.md` as the starting point for `docs/frontend/design.md`.

## Standard frontend design overview

1. Purpose, scope, status, and related docs
2. Users and key journeys
3. Screen/route inventory
4. UX principles and design constraints
5. Frontend architecture and feature boundaries
6. State/data flow and API dependencies
7. Shared components and design tokens (if applicable)
8. Responsive and accessibility requirements
9. Security/privacy considerations
10. Testing and quality gates
11. Assumptions, risks, and open questions
12. Decisions and change history

## Optional focused documents

Create these only when they have a clear audience or maintenance purpose:

- `ui-flows.md`: complex journeys, navigation, alternate/error paths, role-based experiences.
- `frontend-architecture.md`: module boundaries, state/data flow, routing, shared infrastructure.
- `design-system.md`: tokens, components, variants, usage rules, accessibility behavior.
- `frontend-testing.md`: test levels, critical journey coverage, tooling, quality gates.
- ADRs: consequential decisions that are costly to reverse.

## Feature-level technical design

For a significant feature or refactor, a TDD-like note may include:
1. Problem and expected user outcome
2. Scope and acceptance criteria
3. Screens/components/routes affected
4. State and data flow
5. API contract and error cases
6. Accessibility and responsive behavior
7. Security/privacy implications
8. Test plan
9. Rollout, compatibility, and risks

Do not duplicate the PRD or canonical API schema. Link to them and describe only frontend-specific implications.
