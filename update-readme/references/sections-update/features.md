# Update Features Section

How to keep the Features list accurate and complete.

## What to Verify

1. **All features listed** - Match actual capabilities
2. **Feature flags** - Documented if configurable
3. **Removed features** - Marked as deprecated/removed
4. **New features** - Added
5. **Feature descriptions** - Accurate

## Update Process

### Step 1: Extract Features from Code

```bash
# Git tags for releases
git tag -l "v*" | sort -V | tail -5

# Compare changelogs
git log --oneline --all --grep="feature" -20
```

### Step 2: Compare with README

```bash
# Get features from README
grep -E "^[-*] " README.md | grep -v "Installation\|Usage\|API\|License"

# Get from CHANGELOG
grep -E "^## \[v[0-9]" CHANGELOG.md -A20 | grep -E "^- |^### "
```

### Step 3: Sync with CHANGELOG

```markdown
## Features

- Feature A (added in v1.1)
- Feature B (added in v1.0)
- Feature C (added in v1.2)
```

## Feature Categories

### Core Features

Essential functionality:

```markdown
## Features

- **Fast processing** - Handles 10k+ items/second
- **Type-safe** - Full TypeScript support
- **Zero config** - Works out of the box
```

### Advanced Features

Optional capabilities:

```markdown
### Advanced

- Custom validators
- Plugin system
- Debug mode
```

### Enterprise Features

Paid/enterprise only:

```markdown
### Enterprise

- SSO integration
- Audit logging
- Priority support
```

## Common Updates

### Add New Feature

```markdown
## Features

- Existing feature

- **New feature** - Description
```

### Mark as Deprecated

```markdown
## Features

- Feature A - Description

> [!WARNING]
> **Deprecated** in v2.0. Use Feature B instead.
```

### Remove Feature

```markdown
## Features

- Current feature

---

## Removed Features

The following were removed in v2.0:

- Old feature (use NewFeature instead)
```

## Feature Checklist

- [ ] All current features listed
- [ ] No removed features shown as active
- [ ] New features added with version
- [ ] Feature descriptions accurate
- [ ] Categorization correct

## Sync with CHANGELOG

```bash
# Extract features from last release
git log --oneline -30 | grep -i "feat\|feature"

# Get version
git describe --tags --abbrev=0
```

## Template

```markdown
## Features

### Core
- Feature 1 - Description
- Feature 2 - Description

### Configuration
- Feature 3 - Description (configurable via option)

### Optional
- Feature 4 - Description (requires premium)

---

## Roadmap

- [ ] Planned feature A
- [x] Feature B (done in v1.2)
```
