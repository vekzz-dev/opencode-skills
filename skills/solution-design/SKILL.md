---
name: solution-design
description: "Trigger: solution design, diseño de la solución, diseño de software, PRD, SDD, DBDD, TDD, ADR, architecture, data model, requirements, MVP, greenfield. Guide software engineering from idea to implementation, adapting documentation to risk."
metadata:
  author: "vekzz-dev"
  version: "2.2.0"
  language: "en"
  license: "MIT"
---

# Solution Design

An instruction contract for guiding the engineering and design of new or existing software projects. It helps move from the problem and requirements to a verifiable design and a delivery plan, keeping documentation proportional to the project's size, complexity, risk, and way of working. Document types describe responsibilities; **they do not imply that every type must become a separate file**.

## Activation Contract

Load this skill when the user:

- starts a new project or feature and needs to decide the design;
- asks to document architecture, data, API, requirements (PRD), decisions (ADR), or a delivery plan;
- asks for "software design", "PRD", "SDD", "design doc", "document the system", or similar;
- wants to align design with an existing repository.

Do not load for: writing code with no design decision to document, or PR reviews.

## Hard Rules

1. **Start from intent.** First determine whether the user needs to discover requirements, define the product, design architecture, resolve a decision, document data, or prepare implementation.
2. **Inspect before writing.** If a repository exists: review structure, documentation, configuration, models, migrations, tests, and related code. Never replace existing documentation without understanding it.
3. **Never invent.** Do not invent business rules, tables, fields, endpoints, volumes, performance targets, roles, permissions, or controls. Explicitly separate verified facts, accepted decisions, proposals, assumptions, and open questions. Unknown but important things stay marked as "Pending definition".
4. **Never assume an architecture.** Do not recommend microservices, patterns, frameworks, technologies, or infrastructure without sufficient context; explain trade-offs for impactful decisions.
5. **Minimum sufficient documentation.** Do not generate every artifact by default; omit irrelevant sections. Combine before you fragment: on small projects, an integrated `project-design.md`. Separate only for distinct consumers, inherent complexity, versioning, reuse, or security/audit needs.
6. **One source of truth.** A fact, contract, or decision has one canonical location; other documents link to it or summarize it without copying.
7. **Iterative design.** Do not demand exhaustive design before coding: reduce the relevant risks, implement verifiable increments, update as understanding grows.
8. **Document decisions, not every code detail.** Do not duplicate code or describe every class without a real need.
9. **Never confuse documenting with validating.** Do not claim you reviewed code, ran tests, or verified requirements if you did not.
10. **Language and format.** Reply in the user's language, Markdown by default, respect repository conventions.

## Decision Gates

### Documentation level

| Observed signals | Level | Artifacts |
|---|---|---|
| Bounded domain, few integrations, few people, manageable failure | **Lightweight** | `project-design.md` (requirements+data+API+architecture as sections) + `README.md`; OpenAPI only if third parties consume the API |
| Several modules/flows, several collaborators, relevant integrations | **Standard** | `prd.md` + `system-design.md`; `database-design.md` and `openapi.yaml` only if they add independent detail; TDDs for complex changes, ADRs for relevant decisions |
| Multiple teams, complex domain, demanding compliance/security/privacy, strict availability, delicate migrations | **Rigorous** | PRD, SDD, DBDD, API contracts, TDDs, ADRs, test strategy, deployment, observability, traceability |
| Small size but high risk in one area | **Mixed** | Rigor only in the critical areas; the rest lightweight |
| Insufficient context | **Lightweight first** | Record assumptions and expand when a concrete need justifies it |

Factors (evaluate qualitatively; size alone never decides): domain complexity and uncertainty, failure impact and recoverability, data sensitivity/integrity/volume, external integrations, security/privacy/compliance/availability/performance, number of collaborators, and the probability and cost of change. Adjust the level as you learn; communicate only a brief justification when you materially change the deliverables.

### Combine or separate an artifact

| Factor | Combine in an integrated document | Separate |
|---|---|---|
| Readers/consumers | Same, few | Distinct or independently reviewed |
| Changes and ownership | Change together, same owner | Different cadence or owner |
| Needs versioning, tool-based validation, or publishing | No | Yes |
| Complexity/size | Fits in a section | Deserves standalone navigation |
| Traceability/audit/access control | Not required | Required |

Rules: **combine first; separate only when a factor in the "Separate" column is present.** Never create empty files or standalone files just to follow a template.

### Handoff to domain skills

| Question | Owner |
|---|---|
| Cross-cutting process: levels, artifact organization, discovery | This skill |
| API craft: concrete API design rules (resource naming, errors, pagination) | The `api-design` skill, if available |
| Database craft: normalization, indexes, migrations, engine | The `database-design` skill, if available |
| Frontend craft: components, state, UX, accessibility, UI testing | The `frontend-engineering` skill, if available |

## Specialized rules (only when the topic applies)

For data (DBDD, ERD, migrations, engine), users/roles/permissions (functional access requirements vs RBAC/JWT technical strategy), and detailed technical design (diagrams, TDD, OpenAPI, ADR, measurable NFRs): load [`references/specialized-rules.md`](references/specialized-rules.md) and apply only what applies. Floor rules summary: never invent roles/permissions or access rules; functional access requirements belong in the PRD, technical strategy in the SDD; a documented restriction is neither implemented nor verified.

## Execution Steps

1. **Determine the starting point.**
   - New project → greenfield workflow: load [`references/greenfield-workflow.md`](references/greenfield-workflow.md). Start from problem, users, expected outcome, and constraints; never generate architecture, tables, or endpoints before understanding the need.
   - Existing project → inspect the repository; treat code, configuration, tests, and migrations as evidence of the current state. Distinguish current state vs proposed design vs future work.
   - New feature → review context, conventions, and existing designs.
   - Missing non-blocking information → record assumptions and open questions. Ask the user only when ambiguity materially blocks a decision.
2. **Classify need and level.** Load [`references/document-types.md`](references/document-types.md) to identify responsibilities (type ≠ file). Apply the "Documentation level" Decision Gate (criteria above).
3. **Gather available context.** Inspect relevant files; mark important unknowns as "Pending definition" (do not block work on minor details). For decisions affecting architecture, security, persistence, interoperability, or change cost: explain alternatives and trade-offs proportionate to the level.
4. **Choose the documentation organization.** The minimum structure that serves the project's users; example layouts per level are in [`references/templates.md`](references/templates.md). If a repository convention already exists, respect it unless there is a clear reason to change it.
5. **Write or update.** Use [`references/templates.md`](references/templates.md) as guidance, never as a mandatory form. Omit irrelevant sections. When combining, keep clear headers that preserve distinct purposes without duplicating content.
6. **Verify coherence.** Requirements/rules/criteria without contradictions; domain-data-interfaces-flows coherent; endpoints/DTOs/errors/HTTP codes match their canonical sources; links, diagrams, and references valid (never invent destinations); facts/decisions/proposals/assumptions/questions separated.
7. **Prepare incremental delivery.** Where applicable, break the design into small milestones or vertical slices with acceptance criteria and verifiable tests. Never invent dates or precise estimates. Flag blockers and pending high-impact decisions.
8. **Deliver per the Output Contract** (below).

## Output Contract

At the end, report:

1. Files created or modified.
2. Why that documentation level was chosen (and where each responsibility landed if artifacts were combined).
3. The most important assumptions.
4. Pending decisions and blockers.
5. The recommended next increment.

## Quality Criterion

Documentation is ready when it helps understand what must be built or how the existing system works, identify the important decisions and constraints, locate contracts, and verify requirements. File count and length are not quality indicators. **Prefer the simplest structure that keeps the design comprehensible, verifiable, and easy to update.**

For the underlying reasoning (why this skill exists, anti-bureaucracy, why each rule), load [`references/philosophy.md`](references/philosophy.md) when you need to justify or adapt decisions where a rule does not cover the concrete case.

## References

- [`references/greenfield-workflow.md`](references/greenfield-workflow.md) — iterative flow for new projects (discovery → requirements → domain → design → delivery).
- [`references/document-types.md`](references/document-types.md) — responsibilities of each artifact type (PRD, SDD, TDD, DBDD, OpenAPI, ADR) and when to separate.
- [`references/templates.md`](references/templates.md) — orientative templates per level (integrated lightweight, PRD, SDD, DBDD…) and separation criteria.
- [`references/specialized-rules.md`](references/specialized-rules.md) — specific rules for data, users/roles/permissions, and technical design.
- [`references/philosophy.md`](references/philosophy.md) — underlying reasoning, motivation, and v1.2.1 mapping (why behind each rule).
