---
name: update-readme
description: >
  Keep README.md files up-to-date with automatic detection of outdated content and automated updates.
  Trigger: When user asks to update README, detect outdated content, or maintain documentation.
license: Apache-2.0
metadata:
  author: vekzz-dev
  version: "1.0"
---

## When to Use

- Updating existing README.md files
- Detecting outdated documentation
- Maintaining project documentation
- User asks to "update README" or "check documentation"

## Instructions

### When to Update

#### Trigger Events

Update the README when:

1. **Before release** - New version, feature, or fix
2. **API changes** - New functions, removed methods, signature changes
3. **Dependency updates** - Major version bumps (e.g., Node.js 18 → 20)
4. **Breaking changes** - Configuration, behavior, or API changes
5. **Monthly review** - Scheduled maintenance
6. **After feedback** - Users report confusion

#### Warning Signs

- "Requires Node.js 12+" but project uses Node 20
- Installation command doesn't work
- Examples throw errors
- API documentation doesn't match code
- Broken badges (red/404)

### Step 1: Detect What Needs Updating

Use [references/detection/](references/detection/) to find outdated content:

1. **[detect-changes.md](references/detection/detect-changes.md)** - Compare code vs README
2. **[compare-sections.md](references/detection/compare-sections.md)** - Section-by-section verification
3. **[outdated-detection.md](references/detection/outdated-detection.md)** - Common outdated patterns

### Step 2: Update Sections

See [references/sections-update/](references/sections-update/) for each section:

- **[installation.md](references/sections-update/installation.md)** - Package manager commands
- **[usage.md](references/sections-update/usage.md)** - Code examples
- **[api.md](references/sections-update/api.md)** - Function signatures
- **[features.md](references/sections-update/features.md)** - Feature list
- **[badges.md](references/sections-update/badges.md)** - Status badges
- **[requirements.md](references/sections-update/requirements.md)** - Version requirements

### Step 3: Use Checklist

Always verify with [references/checklist.md](references/checklist.md):

- [ ] Installation commands work
- [ ] Usage examples run without errors
- [ ] API documentation matches code
- [ ] Badges show current status
- [ ] Requirements are current

### Step 4: Automate Updates

Set up [references/automation/](references/automation/) for continuous maintenance:

- **[github-actions.md](references/automation/github-actions.md)** - Auto-update workflows
- **[dynamic-badges.md](references/automation/dynamic-badges.md)** - Live stats badges
- **[readme-lint.md](references/automation/readme-lint.md)** - Automated checking

## Commands

```bash
# Check package.json version
cat package.json | grep '"version"'

# Check required Node.js
cat package.json | grep '"engines"'

# List all dependencies
npm list --depth=0

# Check if badges work
curl -I https://img.shields.io/npm/v/package-name

# Check for outdated dependencies
npm outdated

# Check for broken links in README
npx markdown-link-check README.md

# Lint README
npx markdownlint README.md
```

## Quick Detection Checklist

Run these commands to detect issues:

```bash
# Check package.json version
cat package.json | grep '"version"'

# Check required Node.js
cat package.json | grep '"engines"'

# List all dependencies
npm list --depth=0

# Check if badges work
curl -I https://img.shields.io/npm/v/package-name
```

## Common Issues

See [references/common-issues.md](references/common-issues.md) for solutions to:

- Outdated version numbers
- Broken installation commands
- Non-working code examples
- Missing API methods
- Incorrect badges
- Old requirements

## Integration with create-readme

Use [create-readme](../create-readme/SKILL.md) to:

1. **Initial creation** - Generate a new README
2. **Major changes** - Regenerate entire README
3. **New project types** - Different templates

Use this skill (update-readme) for:

1. **Incremental updates** - Keep current
2. **Verification** - Check accuracy
3. **Automation** - Set up auto-updates

## Best Practices

### Always

- Test installation commands
- Run code examples
- Verify API matches implementation
- Check badge URLs
- Update version numbers

### Never

- Copy-paste without testing
- Trust old documentation
- Ignore broken badges
- Skip version bumps
- Leave outdated requirements

### Automation

1. Set up GitHub Actions for stats
2. Use dynamic badges
3. Add README linter to CI
4. Schedule monthly reviews
5. Monitor for feedback

## Resources

- [Detection](references/detection/) - Find outdated content
- [Sections Update](references/sections-update/) - Update specific sections
- [Automation](references/automation/) - Auto-update setup
- [Checklist](references/checklist.md) - Verification list
- [Common Issues](references/common-issues.md) - Problem solutions
