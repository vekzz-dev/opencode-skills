# Frontend Project Workflow

The documentation levels and artifact choices live in the Decision Gates of `SKILL.md` (single source of truth). This reference adds the lifecycle workflows and the criteria for reassessing or consolidating documentation.

## Greenfield workflow

1. **Discover:** understand the problem, users, main tasks, device context, constraints, and success criteria.
2. **Scope:** define the first useful release, exclusions, user-facing requirements, and acceptance criteria.
3. **Map journeys:** identify navigation, screens, alternate paths, and failure/recovery states.
4. **Choose architecture:** select component boundaries, routing, state/data flow, styling, and API integration based on the actual needs and stack.
5. **Set UX and quality expectations:** responsive behavior, accessibility, performance, privacy, browser support, and testing.
6. **Plan vertical slices:** prioritize a complete end-to-end user capability and its tests.
7. **Implement and learn:** build, verify with users or stakeholders where possible, then refine requirements and design.

Do not treat these as a rigid waterfall. Revisit earlier steps when evidence or requirements change.

## Existing project workflow

1. Inspect the repository and its documentation before proposing a new structure.
2. Map current routes, feature modules, component patterns, state/data access, styling, tests, and API usage.
3. Identify the smallest safe change and any architecture/design-system constraints.
4. Compare proposed changes with existing conventions; explain meaningful deviations.
5. Implement the change in a focused increment and run relevant checks.
6. Update only documentation affected by the change.

## Selecting the level

Apply the Decision Gate "Documentation level" in `SKILL.md` to signals such as scope, number of views and flows, state complexity, integration count, accessibility/performance/compliance obligations, risk of failure, and team size. The level can differ per area; increase rigor only where risk warrants it.

## Reassessment triggers

Increase documentation depth when:
- additional teams or independently owned feature areas appear;
- UI behavior becomes inconsistent across features;
- the frontend has complex state synchronization or offline behavior;
- accessibility, privacy, or performance failures would have high impact;
- API contracts are unstable or versioned independently;
- major architecture decisions become expensive to reverse.

Reduce or consolidate documentation when separate files repeat information, no longer have distinct owners or purposes, or impose more maintenance than value. Preserve critical decisions and constraints while consolidating.
