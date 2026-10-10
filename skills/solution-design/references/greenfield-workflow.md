# Engineering flow for a new project (greenfield)

This flow turns an idea into requirements, a verifiable design, and a delivery plan. It is not a rigid waterfall: it adapts to risk and uncertainty and may return to earlier steps when something new is learned. **Documentation is proportional to the project; creating a file per artifact type is not mandatory.**

## 0. Select an initial documentation level

Use the factors from the skill: domain complexity, failure impact, security and data sensitivity, integrations, number of collaborators, uncertainty, and cost of change.

- **Lightweight:** a bounded, low-risk project. Usually a `project-design.md` and a `README.md` are enough.
- **Standard:** several modules, relevant flows, integrations, or collaboration. Separate PRD and system design; separate data/API only if it helps consumers or evolution.
- **Rigorous:** a large, critical system or one with strong compliance, security, availability, or traceability requirements. Separate artifacts for independent review and ownership.

The level can vary by area: a small project with sensitive data may need rigorous security and lightweight documentation for the rest. Start with the lowest level that controls known risks and expand when a concrete reason appears.

## 1. Problem discovery

Define, with the available level of detail:

- The problem or opportunity to solve.
- Users or actors involved and their needs. Determine whether there are user types with different capabilities, anonymous/authenticated access, or resource-ownership rules.
- Expected outcome and success signals; never invent numeric metrics.
- Context, known constraints, dependencies, and boundaries.
- What is explicitly out of the initial scope.

**Output:** a context-and-objectives section in `project-design.md`, or an independent brief/PRD if the scope or collaborators require it. If the idea is still vague, present an initial hypothesis and the priority questions.

## 2. Requirements and MVP scope

Capture, as applicable:

- Users, use cases, and main flows.
- Roles, functional permissions, and access restrictions, if there are relevant differences: who can view, create, modify, delete, or administer which resources. Include a matrix only when it helps validate requirements.
- Functional requirements and business rules.
- Observable acceptance criteria.
- Quality requirements and relevant constraints (security, privacy, performance, availability, accessibility, maintainability, or compliance).
- MVP scope, exclusions, dependencies, risks, and open questions.

Never use vague requirements like "fast", "secure", or "scalable" without explaining how they will be evaluated. Never turn a preferred technical solution into a product need. On a small project, these elements can be brief sections of the integrated document.

## 3. Model the domain and processes

Identify concepts, ambiguous terms, actors, states, transitions, and business invariants. Use a glossary or flow diagrams only when they help clarify logic.

Never automatically turn every noun into a table or class, and never assume the domain model matches the persistence schema exactly.

## 4. Design sufficient architecture

Define the responsibilities and boundaries needed to implement the MVP. Consider, when applicable:

- Components or modules, dependencies, and architectural style.
- Integrations and trust boundaries.
- How role/permission functional requirements will be technically enforced (for example, in the API layer, services, or access policies), identity, and protection of sensitive data. Never confuse capabilities demanded by the business with the technology chosen to enforce them.
- Deployment, configuration, error handling, and observability.
- Maintainability, operational complexity, and evolution risks.

Explain relevant trade-offs. Avoid adopting microservices, cloud, frameworks, or patterns without context. Record high-impact decisions that deserve preserving their reasoning in an ADR — not every trivial choice.

## 5. Design the necessary data and contracts

Describe what is needed to implement and validate:

- **Data:** entities, relationships, key attributes, constraints, and relevant indexes. Add an ERD, a detailed dictionary, or an independent DBDD if complexity or multi-person work justifies it.
- **API:** operations, parameters, schemas, authentication, and errors. Use OpenAPI when the contract must be shared, validated, or maintained as a specification.
- **Integrations:** contracts, dependencies, expected failures, and retries where they correspond.

On a small project, a concise data model and the main endpoints can live in `project-design.md`. Never force contracts or specialized files that are not yet useful.

## 6. Define quality, security, and validation

Tie acceptance criteria to their verification method. Select unit, integration, contract, end-to-end, performance, security, or accessibility tests according to the risks. Add logging, metrics, traces, backups, and recovery when they are real needs.

Never add generic control checklists without considering context; also never omit important controls just because the project is small.

## 7. Plan the first increment

Propose the first vertical slice or milestone that validates a useful part of the system. For each work item, detail as applicable the outcome, scope, dependencies, acceptance criteria, tests, and pending decisions.

Never invent dates or precise estimates. Avoid splitting work into artificially small tasks.

## 8. Review before implementing

Summarize:

- Which requirements and constraints are agreed.
- Which documentation level you chose and why.
- Which architecture and data model you propose.
- Which contracts and quality controls are necessary.
- Which assumptions and high-impact decisions remain open.
- Which files you created and where the canonical source of each subject lives.
- What the first increment is and how it will be verified.

Never claim the project is "fully designed" while important decisions remain unresolved. The review enables an informed start; it does not freeze the design forever. Adjust the affected sections or documents as understanding changes.

## Orientative deliverables per level

### Lightweight

```text
docs/
└── project-design.md
README.md
```

`project-design.md` gathers, as applicable: problem and scope, MVP/requirements, business rules, architecture decisions, data model, API/integrations, test strategy, risks, and open questions. `README.md` covers project setup and execution.

### Standard

```text
docs/
├── prd.md
├── system-design.md
├── database-design.md   # if it needs independent detail
├── api/openapi.yaml     # if the contract justifies it
└── adr/                 # relevant decisions
```

Use only the necessary files. Add a TDD for a feature or change with substantial technical decisions.

### Rigorous

Separate PRD, SDD, DBDD, contracts, TDD, ADR, testing, deployment, and operations according to traceability, audit, collaboration, and independent evolution needs. Keep one source of truth and link between documents.

The goal at every level is the same: reduce ambiguity, control risks, and make implementation easier. What changes is the depth and the organization — not the need to reason about requirements and design.
