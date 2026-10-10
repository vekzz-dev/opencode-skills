# Philosophy and underlying reasoning

Supporting reference for `SKILL.md`. Contains the motivation and reasoning behind the rules. Use it when you need to justify a decision, adapt a rule to a case it does not cover, or explain the approach to the user. It adds no new requirements.

## The real goal: less ambiguity, not more documents

The goal is to reduce ambiguity and enable safe decisions, not to produce the largest possible amount of documentation. Engineering can be rigorous without bureaucracy: on a small project, one well-organized document can hold the essential elements of the PRD, SDD, and DBDD. A 40-page PDF nobody reads and that drifts away from the code is dead documentation; a living, verifiable, updated page is worth more.

## Types describe responsibilities, not files

A common mistake is treating the artifact list (PRD, SDD, DBDD, TDD, ADR) as a list of mandatory files. It is a list of **content responsibilities**. A small project fulfills the PRD with a section of the integrated document; a large system separates the same content because there are different reviewers, ownership, and evolution. Always ask: what question does this artifact answer, and who reads it? Per-type details are in `document-types.md`.

## Rigor ≠ volume

Even at the rigorous level, never fill templates without purpose. Rigor means the relevant decisions and controls are defined and verifiable, not that every topic has its own file. A well-documented critical system demonstrates its rigor with traceable decisions and verifiable controls, not with length.

## Why "never invent" is central

A documented invention becomes reference truth for whoever cannot verify it: someone implements the table that never existed, or assumes a performance target nobody set. That is why the facts / decisions / proposals / assumptions / questions separation is not decorative formatting: it is the mechanism that prevents manufacturing certainty. In discovery, the same applies to users, actors, roles, and permissions: document what is confirmed, mark the unknown as pending, and if there are no relevant access differences, do not force a matrix.

## Why never assume an architecture

Recommending microservices, an ORM, a framework, or infrastructure without context imposes a technical budget on someone who has not yet defined the problem. Architecture decisions have asymmetric reversal costs: the bigger the bet and the less context, the costlier the error. Explain trade-offs in proportion to the decision's real impact.

## Structure follows consumers (not the template)

The criterion for separating artifacts is not complexity itself, but who consumes the information and how: different reviewers, versioning, tool-based validation, independent ownership, auditing. When those factors are absent, separation only adds navigation, duplication, and desynchronization. That is why the level is chosen by risk factors (domain complexity, failure impact, data, integrations, collaborators) and not by size.

## Documenting ≠ validating, iterating ≠ not designing

Two symmetric confusions:

- Believing that writing the design validates it: only code, tests, or review verify. An unimplemented, untested document is a hypothesis.
- Demanding exhaustive design before coding: design exists to reduce relevant risks, not to eliminate them all. Design enough, implement verifiable increments, update as you learn.

## Original 12 principles (v1.2.1) — mapping to current rules

The v1.2.1 principles were condensed into the Hard Rules; this is the audit trail:

| v1.2.1 | Now |
|---|---|
| 1. Start from intent | Hard Rule 1 |
| 2. Inspect before writing | Hard Rule 2 |
| 3. Never invent facts | Hard Rule 3 (+ philosophy above) |
| 4. Adapt rigor to risk | Decision Gate "Documentation level" |
| 5. Minimum sufficient documentation | Hard Rule 5 |
| 6. Combine before fragmenting | Hard Rule 5 + Decision Gate "Combine or separate" |
| 7. One source of truth | Hard Rule 6 |
| 8. Design iteratively | Hard Rule 7 (+ philosophy above) |
| 9. Document decisions, not every detail | Hard Rule 8 |
| 10. Never assume the architecture | Hard Rule 4 |
| 11. Adapt language and conventions | Hard Rule 10 |
| 12. Documenting ≠ validating | Hard Rule 9 |
