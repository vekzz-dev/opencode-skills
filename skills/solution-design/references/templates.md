# Orientative templates

Use these templates as skeletons, never as mandatory forms. Omit irrelevant sections and never fill gaps with invented information. Pick the integrated template for lightweight projects and the specialized templates when separating artifacts adds value.

## Lightweight level: integrated Project Design Document

Use it as a starting point for small, low-risk projects. It is a single-source-of-truth template; you do not need to complete every section before starting.

```markdown
# Project Design — <name>

## 1. Summary
- Problem it solves:
- Main users/actors:
- Expected outcome:
- Document status: Draft / In Review / Accepted

## 2. MVP scope
### Included
### Excluded

## 3. Requirements and business rules
- Functional requirements:
- Important rules:
- Acceptance criteria:
- Relevant constraints and quality requirements:

## 4. Users, roles, permissions, and access restrictions (if applicable)
- User/actor types and needs:
- Confirmed business or system roles:
- Allowed actions and restrictions per role:
- Roles and permissions matrix, if it adds clarity:
- Anonymous/authenticated user access and resource-ownership rules, if applicable:
- Open questions; never assume roles or permissions without evidence:

## 5. Solution design
- Architecture and main components:
- Responsibilities and main flow:
- Technical authentication/authorization strategy (or reference the security design):
- Technologies already decided and reasons, if known:

## 6. Data (if applicable)
- Main entities and relationships:
- Important attributes/constraints:
- ERD diagram if it adds clarity:
- Database engine and migrations, if known:

## 7. API and integrations (if applicable)
- Main interfaces/endpoints:
- Input/output contracts:
- Functional access requirements per operation; technical implementation details go in the technical design:
- External dependencies and relevant failures:

## 8. Security and operations (risk-dependent)
- Authentication, technical authorization, and sensitive data:
- Configuration, deployment, logging/backups if applicable:

## 9. Testing and validation
- How acceptance criteria are verified, including relevant permissions:
- Priority tests:

## 10. Initial plan
- First vertical increment:
- Dependencies and risks:

## 11. Assumptions, pending decisions, and references
- Confirmed:
- Proposed:
- Pending definition:
```

Adapt, delete, or rename sections. If there is no database, delete the data section; if there is no API, delete that section. If no distinct roles or access restrictions exist, omit the permissions matrix and note briefly that it does not apply only when that clarification is useful. Do not document every endpoint or column when that level of detail is not yet needed. In the PRD and in the integrated document, record access capabilities and restrictions from the functional point of view; leave the technical mechanisms (for example, RBAC/ABAC, JWT, sessions, or framework configuration) to the system design or the TDD.

## Common metadata for separated documents

```yaml
# Can be a table if the project does not use frontmatter.
title: "<document name>"
status: "Draft | In Review | Approved | Superseded"
version: "<version or revision>"
last_updated: "<ISO 8601 date>"
owners: []
related_documents: []
```

## PRD

```markdown
# Product Requirements Document — <product>

## 1. Summary and problem
## 2. Objectives and success metrics
## 3. Users, actors, and needs
## 4. Roles, functional permissions, and access restrictions (if applicable)
## 5. Scope and exclusions
## 6. Flows and user stories
## 7. Functional requirements
## 8. Business rules
## 9. Quality requirements and constraints
## 10. Acceptance criteria
## 11. Dependencies and risks
## 12. Open questions
## 13. Related documents
```

## System Design Document

```markdown
# System Design Document — <system>

## 1. Purpose, scope, and current state
## 2. Context and constraints
## 3. High-level architecture
## 4. Components and responsibilities
## 5. Main flows
## 6. Interfaces and integrations
## 7. Data model (summary and reference to the DBDD, if one exists)
## 8. Security
## 9. Quality attributes and known objectives
## 10. Deployment and operations
## 11. Architectural decisions and trade-offs
## 12. Risks and open questions
## 13. Related documents
```

## Database Design Document

```markdown
# Database Design Document — <system>

## 1. Purpose, scope, and known engine
## 2. Conventions and terminology
## 3. Conceptual model
## 4. Logical model and ERD
## 5. Physical model
### Table: <name>
| Column | Type | Nullable | Default | Key/constraint | Description |
|---|---|---|---|---|---|
## 6. Relationships and cardinalities
## 7. Constraints and data integrity
## 8. Indexes and relevant queries
## 9. Data dictionary
## 10. Audit, privacy, and retention (if applicable)
## 11. Transactions and concurrency (if applicable)
## 12. Migrations and change compatibility
## 13. Assumptions, risks, and open questions
## 14. Related documents
```

## Technical Design Document (for a significant change)

```markdown
# Technical Design Document — <feature/change>

## 1. Summary
## 2. Context and technical problem
## 3. Relevant requirements and constraints
## 4. Proposed design
## 5. Affected components and flow
## 6. API/contract changes
## 7. Persistence and migration changes
## 8. Errors, security, and observability
## 9. Alternatives and trade-offs
## 10. Test strategy
## 11. Deployment, compatibility, and rollback
## 12. Risks and open questions
## 13. Related documents
```

## Architecture Decision Record

```markdown
# ADR <number>: <decision>

- Status: Proposed | Accepted | Rejected | Superseded
- Date: <date>
- Decision makers: <people/team, if known>

## Context
## Options considered
## Decision
## Positive and negative consequences
## Risks
## References
```

## Review before delivery

- [ ] The structure and number of files are proportional to the context and risk.
- [ ] The document states purpose, scope, and status.
- [ ] Names and decisions match the project's canonical source.
- [ ] Assumptions, proposals, and open questions are identified.
- [ ] Facts about existing code were verified in the relevant files.
- [ ] No complete content was duplicated from another source of truth.
- [ ] Diagrams distinguish current state from proposed design.
- [ ] Where applicable, roles, functional permissions, and access restrictions are documented in the requirements and reflected in acceptance criteria/tests; the technical strategy lives in the corresponding design.
- [ ] Links point to existing documents or are marked as pending.
- [ ] Sections and artifacts that add no value were omitted.
