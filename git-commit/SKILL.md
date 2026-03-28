---
name: git-commit
description: >
  Execute git commit with conventional commit message analysis, intelligent staging, and message generation.
  Trigger: When user asks to commit changes, create a git commit, or mentions "/commit".
license: Apache-2.0
metadata:
  author: vekzz-dev
  version: "1.0"
---

## When to Use

- Creating a new git commit
- User asks to "commit changes" or "create a commit"
- User mentions "/commit" command
- Analyzing changes for conventional commit format

## Instructions

### 1. Analyze Diff

```bash
# If files are staged, use staged diff
git diff --staged

# If nothing staged, use working tree diff
git diff

# Also check status
git status --porcelain
```

### 2. Stage Files (if needed)

If nothing is staged or you want to group changes differently:

```bash
# Stage specific files
git add path/to/file1 path/to/file2

# Stage by pattern
git add *.test.*
git add src/components/*

# Interactive staging
git add -p
```

**Never commit secrets** (.env, credentials.json, private keys).

### 3. Generate Commit Message

Analyze the diff to determine:

- **Type**: What kind of change is this?
- **Scope**: What area/module is affected?
- **Description**: One-line summary of what changed (present tense, imperative mood, <72 chars)

### 4. Execute Commit

```bash
# Multi-line (body is mandatory)
git commit -m "$(cat <<'EOF'
<type>[scope]: <description>

<body>

[optional footer]
EOF
)"
```

## Commands

```bash
# Check what's staged
git diff --staged

# Check status
git status --porcelain

# Stage all changes
git add .

# Stage specific files
git add file1 file2

# Commit with message
git commit -m "type(scope): description"

# Multi-line commit
git commit -m "$(cat <<'EOF'
type(scope): description

Body text here

Footer: reference
EOF
)"
```

## Conventional Commit Format

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

**Note:** A blank line is REQUIRED between description and body. Body is mandatory (see exceptions below).

## Commit Types

| Type | Purpose |
|------|---------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Formatting/style (no logic) |
| `refactor` | Code refactor (no feature/fix) |
| `perf` | Performance improvement |
| `test` | Add/update tests |
| `build` | Build system/dependencies |
| `ci` | CI/config changes |
| `chore` | Maintenance/misc |
| `revert` | Revert commit |

## Breaking Changes

```markdown
# Exclamation mark after type/scope
feat!: remove deprecated endpoint

# BREAKING CHANGE footer
feat: allow config to extend other configs

BREAKING CHANGE: `extends` key behavior changed
```

## Best Practices

- One logical change per commit
- Present tense: "add" not "added"
- Imperative mood: "fix bug" not "fixes bug"
- Reference issues: `Closes #123`, `Refs #456`
- Keep description under 72 characters

## Commit Body

**The body is MANDATORY.** Include a blank line between the description and body.

When including a body, focus on **what** changed and **why**, not implementation diary:

**Good body:**
```
Add rate limiting to API endpoints

Users were hitting the API too frequently, causing server strain.
This change implements token bucket algorithm to throttle requests.
```

**Bad body (implementation diary):**
```
Add rate limiting

- Added RateLimiter class
- Created middleware
- Added config file
- Fixed tests
```

### Body Guidelines

- Explain the **reason** for the change
- Describe the **problem** being solved
- Mention **consequences** or trade-offs
- Use paragraphs, not bullet lists
- Wrap at 72 characters

## Footer

Footer is **optional**. Use for:
- Breaking changes: `BREAKING CHANGE: <description>`
- Issue references: `Closes #123`, `Refs #456`

## Git Safety Protocol

- NEVER update git config
- NEVER run destructive commands (--force, hard reset) without explicit request
- NEVER skip hooks (--no-verify) unless user asks
- NEVER force push to main/master
- If commit fails due to hooks, fix and create NEW commit (don't amend)
