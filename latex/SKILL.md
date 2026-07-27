---
name: latex
description: "Trigger: LaTeX, latex, .tex, documento académico, artículo, tesis, apuntes, guías, ensayos. Create compile-safe LaTeX academic documents."
license: MIT
metadata:
  author: "vekzz-dev"
  version: "2.1"
---
# Skill: LaTeX Academic Documents

## Activation Contract

Load when creating, editing, or fixing `.tex` files for academic documents (articles, theses, essays, notes, guides) or when diagnosing compilation errors from `.log` files.

## Hard Rules

1. **`hyperref` MUST be the last package loaded** in the preamble.
2. **Never use `\\` to separate paragraphs** — use blank lines. `\\` is for tables, alignments, and line breaks inside special environments only. Pair with `\usepackage{parskip}` for visual paragraph separation.
3. **Remove file extensions** from `\includegraphics`, `\include`, `\input` — let LaTeX resolve them.
4. **Use `\enquote{text}`** (from `csquotes`) for quotation marks, never raw `"text"`.
5. **Brace command names before text**: `\LaTeX{}` not `\LaTeX`.
6. **Always provide a working compilation command** with every document.
7. **Default fonts are Lora (serif) + Lato (sans)** via `fontspec`. Engine is always lualatex.
8. **For Spanish documents**: `babel[spanish]` (works with lualatex).

## Decision Gates

| Document type | Class | Engine | Fonts |
|---|---|---|---|
| Article, essay, apuntes | `article` | lualatex | Lora + Lato |
| Large article, report, guía | `report` | lualatex | Lora + Lato |
| Thesis (tesis) | `memoir` or `book` | lualatex | Lora + Lato |
| Presentation | `beamer` | lualatex | Lato (sans-default) |

## Execution Steps

1. **Identify document type** from the request, then select class via the Decision Gates. Engine is **always lualatex**.
2. **Build preamble in this order**: `\documentclass[a4paper,12pt]{...}` → `fontspec` → `\setmainfont{Lora}` + `\setsansfont{Lato}` → language (`babel[spanish]`) → math (`amsmath`, `amssymb`, `mathtools`, `amsthm`) → tables (`booktabs`) → graphics (`graphicx` + `\graphicspath{{figures/}}`) → typography (`microtype`) → paragraphs (`parskip`) → code (`listings` or `minted`) → citations (`biblatex` with `biber`) → **`hyperref` LAST**.
3. **If Lora/Lato are not installed**: remove or comment the `\setmainfont{Lora}` / `\setsansfont{Lato}` lines. lualatex will use Latin Modern as the default — the document compiles without changes.
4. **Structure**: `main.tex` is a skeleton. Use `\input` for sections. Use `\include` only for top-level chapters in books/reports.
5. **Bibliography**: `\addbibresource{refs.bib}` in preamble, `\printbibliography` at document end.
6. **Compilation command**: `latexmk -lualatex main.tex`. `latexmk` handles multi-pass and biblatex automatically.
7. **Existing projects**: read the preamble, `.latexmkrc`, and any `.cls`/`.sty` files first — match their conventions instead of overriding them.

## Common Errors & Diagnosis

| Error | Likely cause | Fix |
|---|---|---|
| `! Undefined control sequence. \foo` | Missing package or typo in command name | Add the required `\usepackage{...}` or correct spelling |
| `! LaTeX Error: File 'x.sty' not found.` | Package `x` is not installed | `tlmgr install x` (TeX Live) |
| `! Package hyperref Error: Wrong driver` | `hyperref` loaded before a driver-dependent package | Move `\usepackage{hyperref}` to the **very end** of the preamble |
| `! Missing \endcsname inserted` | Special chars in `\label{}` or `\ref{}` | Use only ASCII and `:` in labels: `\label{sec:intro}` |
| `! Emergency stop.` | Syntax error near the top of the file | Check first 20 lines for unbalanced braces or unmatched `\begin`/`\end` |
| `! Package babel Error: Unknown option 'spanish'.` | Babel installed without Spanish patterns | `tlmgr install babel-spanish` |
| `! LaTeX Error: Option clash for package x` | Package loaded twice with different options | Load once with all options: `\usepackage[opt1,opt2]{x}` |
| **No .pdf, .log truncated** | Infinite loop or out of memory | Check for `\def\a{\a}` loops or conflicting packages |
| **fontspec warnings about Lora/Lato** | Fonts not installed on this system | Remove `\setmainfont{Lora}` / `\setsansfont{Lato}` — lualatex falls back to Latin Modern |

## Output Contract

Return: created/modified `.tex` files, the compilation command, and key structural decisions. If fixing errors, include the root cause from `.log` analysis and which fix was applied.

## Resources

- [latexmk documentation](https://ctan.org/pkg/latexmk)
- [TeX Live package search](https://ctan.org/pkg)
