# Artifact types and responsibility boundaries

Names are not universal across teams. Use these operational definitions and respect the conventions established in the project. **An artifact type is not necessarily a file.** A small project can hold several of these responsibilities as sections of `project-design.md`; separate them when independent review, ownership, versioning, or evolution is required.

## PRD — Product Requirements Document

**Question:** What problem will be solved, for whom, and what must the product do?

Usually includes objective, users/actors and their needs, scope/exclusions, use cases, functional requirements, acceptance criteria, business rules, quality requirements from the product perspective, metrics, constraints, and open questions. **When access differences exist, include functional roles and permissions:** which actions each user type can perform, which restrictions exist, and, if it adds clarity, a roles-and-permissions matrix. Also consider anonymous/authenticated access and resource-ownership rules where appropriate.

Never invent roles, privileges, or authorization requirements unsupported by the context; record the unknown as an open question. If all users have the same capabilities or the system requires no access control, do not force a matrix. The PRD defines required behavior, not the technical mechanism. It must not become a detailed specification of SQL tables, classes, frameworks, or internal architecture. On a small project it can be a section of `project-design.md`.

## SDD — System Design Document

**Question:** How is the system technically organized as a whole?

Usually includes technical context and scope, high-level architecture, components, module/service boundaries, main flows, integrations, security (including the technical authentication and authorization strategy when applicable), quality attributes, deployment, and references to the data/API design.

It can be a section of an integrated document on simple projects. Separate it when its architecture needs independent review or maintenance.

## TDD — Technical Design Document

**Question:** How will we implement a concrete feature or technical change?

Usually includes change context, requirements/constraints, proposed solution, alternatives, affected components, flow, API and data changes, errors, security, testing, deployment, and risks.

Never create a TDD for every small task. Use it when a feature's technical decisions deserve a reviewable proposal. TDD also means *Test-Driven Development*; write out the full term when ambiguity exists.

## DBDD — Database Design Document

**Question:** How are the data modeled, related, constrained, and persisted?

Usually includes known engine, conceptual/logical/physical models, ERD, tables and columns, keys, relationships, constraints, indexes, data dictionary, integrity, migrations, and performance, privacy, retention, or audit considerations when applicable.

On a small project it can be a section or a concise table within `project-design.md`. Separate it if the schema is complex, reviewed separately, has delicate changes, or is consumed by different teams.

## API contract — for example, OpenAPI

**Question:** How do consumers interact with the API?

Defines operations, routes, parameters, request/response schemas, authentication, errors, and HTTP codes. Use OpenAPI as the structured source of truth when the contract needs to be shared, validated, or versioned. It does not always add value as a separate file for a small, locally used API with no independent consumers.

## ADR — Architecture Decision Record

**Question:** Which relevant technical decision was made, why, and what are its consequences?

Records context, relevant options, decision, consequences, and status. An ADR can be brief and is worthwhile when an important decision is hard to reverse, even on a small project. Never record every trivial decision as an ADR.

## README and operational guide

**Question:** How is this repository installed, configured, run, and tested?

The README documents setup and practical use. It must not duplicate the whole system design; link to the canonical design document when needed.

## How to choose what to separate

Keep responsibilities in one integrated document when they are short, change together, have the same readers, and separation only adds navigation or duplication. Separate an artifact when one or more of these factors justify it:

- It has distinct consumers or reviewers.
- It must be validated, versioned, or published with a specialized tool.
- It is complex enough to warrant independent navigation.
- It changes at a different cadence or has independent ownership.
- It requires traceability, auditing, or specific access controls.
- Separating it reduces duplication or inconsistency risks.

Quick questions:

- "What must the product do?" → PRD, or a requirements section.
- "How is the system organized?" → SDD, or an architecture section.
- "How will we implement this complex change?" → TDD.
- "Which tables, relationships, types, and constraints do we need?" → DBDD, or a data section.
- "Which operations and schemas does the API expose?" → API contract/OpenAPI if useful.
- "Why did we choose this architecture or technology?" → ADR if the decision matters.

Combining is valid; duplicating the source of truth is not. Keep names, contracts, and decisions canonical in one place and link from other artifacts.
