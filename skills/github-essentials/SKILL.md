---
name: github-essentials
description: "Trigger: .github setup, community health files, issue templates, PR template, CODEOWNERS, SECURITY.md, dependabot, release workflow. Bootstrap a repository's .github directory."
license: MIT
metadata:
  author: vekzz-dev
  version: "1.0"
---

## Activation Contract

Load when the user asks to set up, complete, or audit a `.github/` directory: issue templates, pull request template, release workflow, CODEOWNERS, SECURITY.md, or dependabot.

Do not load for README authoring (`create-readme`), changelog upkeep (`changelog-maintenance`), or CI pipelines. Only the release workflow is in scope here.

## Hard Rules

- Never write a file the user did not approve. Present the plan and STOP first.
- Never leave a placeholder in a written file. Resolve `__OWNER__`, `__REPO__`, `__CODEOWNER__`, `__SECURITY_EMAIL__`, and `__PACKAGE_ECOSYSTEM__` from repository evidence. If a value cannot be resolved, ask instead of guessing.
- Never write test, lint, build, or registry-publish workflows. Release only.
- Use issue forms (`.yml`), never legacy `.md` issue templates.
- The release workflow triggers on `push: tags` only, never on `workflow_dispatch` or `release`.
- Write templates in the language the repository already uses; default English.

## Decision Gates

| Condition | Action |
|---|---|
| Repository already has a `.github/` file | Read it and propose a merge. Never silently overwrite. |
| Org-wide `.github` repository | Skip `ISSUE_TEMPLATE/config.yml` contact links: they resolve per consuming repository |
| `CODEOWNERS` target unknown | Read `git config user.email` and `git log -1 --format='%an'`; confirm with the user before writing |
| Stack undetected for `dependabot.yml` | Read manifests and lockfiles. If ambiguous, ask |
| User requests registry publishing | Out of scope. Report it and stop |

## Execution Steps

1. Inspect: `git remote get-url origin`, `git symbolic-ref --short refs/remotes/origin/HEAD`, existing `.github/`, lockfiles and manifests, and `git tag --list 'v*'` to infer the current version line.
2. Derive every placeholder value from that evidence. Mark anything unresolved as a question.
3. Present the file list with each resolved value. STOP and wait for approval.
4. Copy the approved files from `assets/` into `.github/`, substituting every placeholder.
5. Validate with `references/validation.md`: leftover placeholders, YAML parse, CODEOWNERS syntax, tag regex against the project's versioning.
6. Report results and the exact command that publishes the first release.

## Output Contract

Return a table of every path written with its one-line purpose, the resolved values (`owner`, `repo`, `codeowner`, `security email`, `ecosystem`), every file skipped and why, and the commands to verify locally:

```bash
grep -rn "__[A-Z_]*__" .github/ || echo "no placeholders left"
python3 -c "import sys,yaml; [yaml.safe_load(open(f)) for f in sys.argv[1:]]" .github/*.yml
```

## References

- `assets/` — one template per `.github` artifact.
- `references/release-workflow.md` — tag-driven release design and failure modes.
- `references/validation.md` — pre-delivery checklist.
