---
name: git-commit
description: >
  Execute atomic git commits with conventional commit message analysis, intelligent staging, and message generation.
  Trigger: When user asks to commit changes, create a git commit, or mentions "/commit".
license: MIT
metadata:
  author: vekzz-dev
  version: "1.2"
---

## When to Use

- Creating a new git commit
- User asks to "commit changes" or "create a commit"
- User mentions "/commit" command
- Analyzing changes for conventional commit format

## Instructions

**Core principle: one commit = one deliverable behavior.** If the working tree has multiple unrelated changes, do NOT commit them together. Commit one, then the next.

### 0. Classify the Changes (MANDATORY)

```bash
# See everything that changed (tracked + untracked)
git status --porcelain

# See full diff
git diff

# Also see staged if resuming a partial commit
git diff --staged
```

Group every change by **which deliverable behavior** it belongs to:

- Bug fix A (e.g., null pointer in login)
- Feature B (e.g., dark mode toggle)
- Refactor C (e.g., extract auth helper)
- Config D (e.g., CI workflow change)

**If 2+ unrelated concerns exist:** stop. Do NOT stage everything. Show the user the groups and ask: *"Which one do you want to commit now?"*

**If only one concern:** proceed directly to step 1.

### 1. Stage This Atomic Change ONLY

Stage **only** the files or hunks that belong to the selected logical change:

```bash
# Stage specific files
git add path/to/file1 path/to/file2

# Stage individual hunks when a file has mixed concerns
git add -p path/to/file-with-mixed-changes

# Stage by pattern
git add src/feature-b/*
```

**Never commit secrets** (.env, credentials.json, private keys). Add these to `.gitignore` preventively.

**Never use bare `git add .`** without first verifying it only captures one concern.

### 2. Verify Atomic Staging (MANDATORY GATE)

```bash
git diff --staged
```

Confirm the staged diff passes **The Review Test**:

- [ ] Is this ONE deliverable behavior?
- [ ] Does the repo compile/build with ONLY this change?
- [ ] Can someone understand what it does from the diff?
- [ ] Are tests included if this adds or changes behavior?

If the staged changes fail any check — stop, unstage (`git restore --staged .`), and re-split.

### 3. Generate Commit Message

Analyze the staged diff to determine:

- **Type**: What kind of change is this?
- **Scope**: What area/module is affected?
- **Description**: One-line summary of what changed (present tense, imperative mood, <72 chars)

### 4. Execute Commit

```bash
# Multi-line (body is mandatory)
git commit -m "$(cat <<'EOF'
<type>(<scope>): <description>

<body>

[optional footer]
EOF
)"
```

### 5. Signed Commits (GPG/SSH)

If you have a GPG or SSH signing key configured:

```bash
# Sign a single commit
git commit -S -m "type(scope): description"

# Or configure Git to sign all commits by default
git config --global commit.gpgsign true
```

GitHub marks signed commits as **Verified**. See [GitHub's signing guide](https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-commits) to set up a key.

## Commands

```bash
# Check what's staged
git diff --staged

# Check status
git status --porcelain

# Stage specific files for an atomic commit
git add src/feature-b/*

# Stage individual hunks (mixed concerns in one file)
git add -p src/file-with-mixed-changes

# Commit with message
git commit -m "type(scope): description"

# Multi-line commit (body is mandatory in this skill)
git commit -m "$(cat <<'EOF'
type(scope): description

Body text here

Footer: reference
EOF
)"

# Unstage everything (keep changes in working tree)
git restore --staged .

# Unstage a single file
git restore --staged path/to/file

# Amend last commit message (before pushing)
git commit --amend -m "type(scope): corrected message"

# Add forgotten files to last commit (before pushing)
git add forgotten-file.ts && git commit --amend --no-edit

# Create fixup commit for later rebase
git commit --fixup HEAD~2

# Autosquash fixup commits during rebase
git rebase -i --autosquash HEAD~5

# Preview the commit without creating it
git commit --dry-run
```

## Conventional Commit Format

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

**Note:** A blank line is REQUIRED between description and body. Body is mandatory — explain the **why**, not just the **what**.

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

## Atomic Commits & Logical Order

### Definition

A commit represents a **deliverable behavior, fix, migration, or documentation unit** — not a file type or architectural layer.

### Logical Order

Commits must follow dependency order so each step is independently verifiable:

1. **Dependencies first** — models, types, schemas, migrations
2. **Behavior after** — services, use cases, business logic
3. **Integration at the end** — wiring, endpoints, UI, configuration

**Tests** belong in the same commit as the behavior they verify.
**Docs** belong in the same commit as the user-visible change they explain.

### The Review Test

Before committing, confirm:

- [ ] Can this commit be understood **on its own**?
- [ ] Does the repo **compile / build** with ONLY this commit applied?
- [ ] Can I **revert** this commit without breaking unrelated work?
- [ ] Are **tests included** if this commit adds or changes behavior?

### Good vs Weak Splits

**Weak — split by file type (avoid):**

```
feat: add User model
feat: add User service
feat: add User controller
feat: add User tests
```

Why: each commit leaves the repo in a broken or incomplete state. Tests arrive after the fact, so nothing is verifiable mid-series.

**Strong — split by behavior, logical order:**

```
feat(auth): add User model with validation rules
feat(auth): implement user registration with tests
feat(auth): wire registration endpoint
```

Why: each commit is self-contained. The repo compiles after every step. `git bisect` works reliably.

### Anti-Patterns

| Anti-Pattern | Why It Fails |
|---|---|
| Tests in a separate commit | Behavior can't be verified until tests arrive — breaks `git bisect` |
| Docs as a follow-up commit | Docs arrive after the feature shipped, confusing users |
| Refactor + feature in one commit | Two concerns: impossible to review or revert cleanly |
| Mid-series compilation broken | Every commit must compile — otherwise the series is not atomic |
| `git add .` without reviewing staging | Groups unrelated changes into one commit, losing atomicity |

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

## Related Skills

| Skill | When to Use |
|-------|-------------|
| `work-unit-commits` | Plan commits as reviewable slices — use **before** writing code |
| `chained-pr` | Split large changes into stacked PRs that protect review focus |

## Resources

- [Conventional Commits specification](https://www.conventionalcommits.org/)
- [Git commit docs](https://git-scm.com/docs/git-commit)
- [GitHub — signing commits](https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-commits)

## Git Safety Protocol

- NEVER update git config
- NEVER run destructive commands (--force, hard reset) without explicit request
- NEVER skip hooks (--no-verify) unless user asks
- NEVER force push to main/master
- If commit fails due to hooks, fix and create NEW commit (don't amend)
