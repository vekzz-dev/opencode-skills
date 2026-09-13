# Note Anatomy — Learning Tech Note

A tech note must let the reader **understand and apply** the topic later, not just look up the API. This anatomy is what makes that happen.

## Sections, in order

| # | Section | What goes in it |
|---|---|---|
| — | TL;DR | One line: what it is and why it matters. |
| 1 | La idea | The mental model — one image that **derives** the rules that follow. No tables, no API. Reading only this should let someone *explain* the topic. |
| 2 | Reference sections | The API: concepts, syntax, operations — each labeled with the version it appeared in. |
| 3 | Ejemplo trabajado | A real problem solved step by step, showing the **reasoning**. |
| 4 | Patrones comunes | Table: need → solution. The lookup layer. |
| 5 | Errores comunes | Symptom → cause → fix. Prefer traps that fail silently. |
| 6 | Pruébate | 5–9 questions with revealable answers. |
| 7 | Referencias | Official sources, linked. |
| 8 | Notas relacionadas | Wiki links, including the domain index. |

## "La idea" — the part that matters most

It must **derive** the rules, not summarize them. Test: can the mental model predict a rule you never stated?

Strong models from real notes:

- **Streams** = an assembly line → nothing moves until the terminal operation; element by element, not stage by stage; stages are pure and single-use.
- **Optional** = absence becomes a contract → it lives in the return type; chain, don't unwrap.
- **java.time** = two questions decide the type (instant or calendar date? which zone?) → everything is immutable.
- **JDBC** = nothing is hidden, every step is explicit → close what you open; `PreparedStatement` separates SQL from data; a transaction is all-or-nothing.

## "Ejemplo trabajado" — show the thinking

An order that works:

1. **The problem**, stated as a real need.
2. **Think first**: what transformations or steps does it need? A table beats prose here.
3. **Translate** each step to code.
4. **Compare** with the naive/imperative version: what was gained, what was lost.
5. **A variation**: change one requirement and show the change is small *if the model is right*.

## "Pruébate" — callout format

```markdown
> [!question]- The question, answerable without looking?
> The answer.
```

Design questions to catch **wrong mental models**, not to test memory. Example: *"the list has 1000 elements and the match is the 3rd — how many times does `f` run?"* (answer: 3, not 1000 — it catches someone still thinking in loops).

## Anti-patterns

- **Catalog without a model.** API lists with no "La idea" produce a reference, not a learning note.
- **Two topics in one note.** If it needs two "La idea" images, it is two notes.
- **Unverified versions.** Never write "since version X" without checking the owning source.
- **A summary disguised as a model.** A "La idea" that restates the sections is noise.
- **Answers with no reveal.** A "Pruébate" whose answers are not collapsible is a test, not study material.
- **Missing index link.** A note not listed in its domain index is invisible to retrieval.
