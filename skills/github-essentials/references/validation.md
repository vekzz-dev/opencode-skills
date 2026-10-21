# Pre-delivery validation

Run every check before reporting the work as done. Each one has an observable pass condition.

## 1. No placeholders survive

```bash
grep -rn "__[A-Z_]*__" .github/ || echo "clean"
```

Any match is a defect. Resolve the value or ask the user.

## 2. Every YAML file parses

```bash
python3 - <<'PY'
import pathlib, sys, yaml
bad = []
for p in pathlib.Path(".github").rglob("*.yml"):
    try:
        yaml.safe_load(p.read_text())
    except yaml.YAMLError as e:
        bad.append(f"{p}: {e}")
print("\n".join(bad) or "all valid")
PY
```

Issue forms have a stricter shape than plain YAML. A file that parses can still be rejected by GitHub when `body` entries lack `type` or `attributes`.

## 3. Issue forms are complete

For each file in `.github/ISSUE_TEMPLATE/`:

- `name`, `description`, and `body` exist at the top level
- `title` is present, or the default title is acceptable
- every `body` entry has `type` and `attributes.label`
- at most one `type: markdown` entry per position, and it is used for instructions only
- `id` values are unique within the file
- `labels` names already exist in the repository, or are created on first use

## 4. CODEOWNERS resolves

- The file lives at `.github/CODEOWNERS` (or `CODEOWNERS` at the root; only one applies)
- Every handle is `@user`, `@org/team`, or an email
- The catch-all `*` is present, and specific paths are listed after it
- Every team referenced grants write access, otherwise the review request silently fails

## 5. Release workflow is coherent

- Trigger is `push: tags` with a pattern matching the project's real tags
- `permissions` contains `contents: write`
- `fetch-depth: 0` is set on checkout
- The tag regex matches the versioning the repository actually uses
- `--verify-tag` is present
- The YAML has no tab characters

## 6. Dependabot ecosystem is real

Valid values include `npm`, `pip`, `maven`, `gradle`, `gomod`, `cargo`, `composer`, `bundler`, `docker`, `github-actions`, `nuget`, `pub`.

`directory` must contain the manifest for that ecosystem. A monorepo usually needs one entry per directory.

## 7. Files do not collide with existing configuration

Read `.github/` before writing. Overwriting an existing `CODEOWNERS` or `dependabot.yml` without asking loses configuration the team added for a reason.
