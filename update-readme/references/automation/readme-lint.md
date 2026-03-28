# README Lint

Automated checking of README quality and correctness.

## Why Lint README

- Catch broken links
- Verify code examples
- Ensure consistency
- Auto-maintain quality

## Linting Tools

### 1. markdownlint

```bash
# Install
npm install -D markdownlint-cli2

# Config .markdownlintrc.json
{
  "MD001": true,
  "MD002": "first-heading-at-level",
  "MD004": false,
  "MD013": false,
  "MD033": false
}
```

### 2. README Linter Action

```yaml
# .github/workflows/readme-lint.yml
name: Lint README

on:
  push:
    paths:
      - 'README.md'
  pull_request:
    paths:
      - 'README.md'

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Lint README
        uses: DavidAnson/markdownlint-cli2-action@v14
        with:
          globs: README.md
```

### 3. Custom Lint Script

```bash
#!/bin/bash
# lint-readme.sh

ERRORS=0

echo "=== README Lint ==="

# Check required sections
for section in "Installation" "Usage" "License"; do
  if grep -q "## $section" README.md; then
    echo "✅ $section section found"
  else
    echo "❌ Missing: ## $section"
    ERRORS=$((ERRORS + 1))
  fi
done

# Check for broken links (sample)
echo ""
echo "Checking links..."
# Add link checking logic

# Check for TODO/FIXME
if grep -q "TODO\|FIXME" README.md; then
  echo "⚠️ TODO/FIXME found in README"
fi

# Check code blocks closed
OPEN=$(grep -c "```" README.md)
if [ $((OPEN % 2)) -ne 0 ]; then
  echo "❌ Unclosed code block"
  ERRORS=$((ERRORS + 1))
fi

echo ""
echo "Errors: $ERRORS"
exit $ERRORS
```

## GitHub Action for Linting

### Complete Workflow

```yaml
name: README Quality

on:
  push:
    branches: [main]
  pull_request:
    paths:
      - 'README.md'

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run linters
        run: |
          # Check links
          npx broken-link-checker --ordered --filter-level 3 --host-asking 3 README.md || true
          
          # Check formatting
          npx markdownlint README.md

      - name: Check content
        run: |
          bash .github/scripts/readme-validate.sh

  build:
    needs: lint
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Verify examples
        run: |
          npm install
          node -e "require('./examples/test.js')"

  test:
    needs: lint
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Test installation
        run: |
          npm install
          npm run build || echo "Build step needed"
```

## Automated Fixes

### Auto-Fix Links

```yaml
- name: Fix links
  run: |
    # Fix common issues
    npx remark-validate-links README.md
    npx remark --use remark-fix README.md
```

### Auto-Format

```yaml
- name: Format README
  run: |
    npx prettier --write README.md
```

## Quality Checks

### Pre-commit Hook

```yaml
# .pre-commit-config.yaml
repos:
  - repo: https://github.com/igorshubovych/markdownlint-cli
    rev: v0.37.0
    hooks:
      - id: markdownlint

  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.5.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
```

### Install pre-commit

```bash
pip install pre-commit
pre-commit install
```

## Lint Rules

### Required

- All links valid
- Code blocks closed
- No broken badges

### Recommended

- H1 at top only
- Clear sections
- Working examples
- Table of contents for long READMEs

### Optional

- Consistent formatting
- Badge style
- Line length

## Custom Linter Rules

### GitHub Action

```yaml
name: README Check

on: [pull_request]

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Custom checks
        run: |
          # Check for outdated versions
          grep -E "Node\.js (1[0-4]|15|16)" README.md && echo "❌ Old Node version"
          
          # Check for TODO
          grep "TODO" README.md && echo "⚠️ TODO in README"
          
          # Check for empty sections
          grep -E "^## .*$" README.md | while read section; do
            lines=$(awk "/$section/,/^## /" README.md | wc -l)
            if [ "$lines" -lt 3 ]; then
              echo "⚠️ Empty or short section: $section"
            fi
          done
```

## Integration with CI

### Full Pipeline

```yaml
name: CI

on: [push, pull_request]

jobs:
  readme-lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Lint README
        run: |
          npx markdownlint README.md

  code-quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: ESLint
        run: npm run lint

  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Test
        run: npm test

  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Build
        run: npm run build

  deploy:
    needs: [test, build]
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v4
      - name: Deploy
        run: echo "Deploy..."
```

## Badge for Lint Status

```markdown
[![README Lint](https://github.com/USER/REPO/actions/workflows/readme-lint.yml/badge.svg)](https://github.com/USER/REPO/actions/workflows/readme-lint.yml)
```
