---
name: vault-tech-note
description: "Trigger: technical note, tech note, vault note, documentation note, nota técnica. Write learning-first tech notes for an Obsidian vault with verified sources."
license: MIT
metadata:
  author: vekzz-dev
  version: "1.0"
---

## Activation Contract

Load when asked to create, extend, or refactor a **technical note** in the user's Obsidian vault, for any language, library, or tool (Java, Docker, Spring, SQL...).

Not for: project plans, design decisions, Kanban boards, or config guides — those have their own note types.

## Hard Rules

- Never state a version, API, or behavior from memory. Verify it against the source that owns the fact.
- One note = one topic. Two topics means two notes, cross-linked.
- Frontmatter in English; body in the user's language; code and identifiers always English.
- Every note links upward to its domain index, and the index links back.
- Never invent the vault's rules. Read the vault `README.md` first: it is the contract.
- Follow `references/note-anatomy.md`. Do not improvise sections.

## Decision Gates

**Which source owns the fact:**

| Fact is about | Source |
|---|---|
| Release status, JEP, "final or preview in which version" | Web search (OpenJDK JEPs, release notes) |
| API, config, or syntax of a **library/framework** | context7 — resolve the ID, pass the target version |
| Language or JDK behavior | Official javadocs (web) |

Cross-check two sources when in doubt.

**One topic or two:**

| Signal | Action |
|---|---|
| Needs two "La idea" images | Split into two notes |
| Two stacks or audiences in one file | Split |

## Execution Steps

1. Read the vault `README.md`; locate the domain index and search for an existing note. Update instead of duplicating.
2. Draft "La idea": the mental model that **derives** the rules below it, not a summary of them.
3. Fill the rest of the anatomy from `references/note-anatomy.md`, starting from the vault's tech-note template, and match the bar in `references/exemplar.md`.
4. Build "Ejemplo trabajado" from a real problem: transformations first, then code, then a variation.
5. Add "Pruébate": 5–9 questions in collapsible callouts designed to catch wrong mental models.
6. Link the note to its domain index and add it to that index.
7. Verify every version and API claim against its owning source, then confirm frontmatter parses, links resolve, and the note covers one topic.

## Output Contract

Return: the note path; its section list; every claim with the source used to verify it; confirmation it was added to its domain index. If a vault rule is unclear, report it instead of guessing.

## References

- `references/note-anatomy.md` — section-by-section spec of a learning tech note.
- `references/exemplar.md` — a condensed real note; the quality bar to match.
- Vault `README.md` — the vault contract (paths, frontmatter, types, linking).
