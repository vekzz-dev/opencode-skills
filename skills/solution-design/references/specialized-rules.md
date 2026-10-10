# Specialized rules: data, users/permissions, and technical design

Domain rules for when the documentation covers persistence, access control, or detailed technical design. Load them only when the project needs them; never force sections that do not apply.

## Data (DBDD or data section)

- Distinguish conceptual, logical, and physical models to the degree complexity requires.
- Document tables/entities, attributes/columns, types, nullability, defaults, keys, and relevant constraints; cardinality and referential integrity. ERD only when it adds clarity.
- Include definitions of important fields; this can be a brief table inside `project-design.md`.
- Document dates and time zones, logical/physical deletion, auditing, sensitive data, transactions, and migrations when applicable.
- Never assume the physical design is portable across engines. State engine and version only if known.
- With an ORM (for example, JPA): distinguish the domain model from persistence and review migrations as evidence of the deployable schema.
- Never create a separate DBDD if a concise section satisfies the project's needs; separate it when complexity, collaboration, review, or independent change justifies it.

## Users, roles, and permissions

| Stage | Rule |
|---|---|
| Discovery | Identify users and actors; determine whether their capabilities or access restrictions differ. Never assume every application needs roles. |
| PRD (or section of `project-design.md`) | Document functional access requirements: which users can perform which actions, on which resources, under which restrictions. Roles-and-permissions matrix only when it facilitates review or acceptance criteria. If there are no relevant differences, omit the matrix. |
| Missing definitions | Record open questions; never invent roles, privileges, or access rules. |
| SDD | Document the technical strategy (RBAC/ABAC, JWT, sessions, framework annotations, table structure). Never mix business needs with implementation decisions. |
| TDD | Detail the authorization implementation when the feature justifies it. |
| Validation | Include permissions in acceptance criteria and authorization tests; never treat a restriction as implemented or verified just because it appears in a document. |

Never invent roles, privileges, or access rules without backing; the unknown is an open question.

## Technical design (SDD/TDD)

- Describe components, boundaries, responsibilities, dependencies, flows, and interfaces at the appropriate level.
- Diagrams (for example, Mermaid) only when they improve understanding and clearly reflect the current state or the proposal.
- ADR for important decisions, even on a small project, when consequences are hard to reverse.
- Separate TDD only for features or changes with significant technical complexity, never for every small task. TDD can also mean *Test-Driven Development*; write out the full term if ambiguous.
- OpenAPI as the structured source of truth only for APIs with consumers or explicit contract requirements; never generate it by default for a trivial internal API.
- Define measurable criteria for non-functional requirements when possible; never invent quantitative targets.
