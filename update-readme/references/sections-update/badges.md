# Update Badges Section

How to keep badges accurate and functional.

## What to Verify

1. **Version badge** - Current version
2. **Build badge** - Passing
3. **License badge** - Correct
4. **Downloads badge** - Working
5. **All URLs** - Not broken

## Update Process

### Step 1: Check All Badges

```bash
# Extract all badge URLs
grep -oP 'https://img\.shields\.io[^")]+' README.md

# Check each badge
for url in $(grep -oP 'https://img\.shields\.io[^")]+' README.md); do
  status=$(curl -s -o /dev/null -w "%{http_code}" "$url")
  echo "$status - $url"
done
```

### Step 2: Generate Current Badges

#### Version Badge

```bash
# npm
VERSION=$(node -p "require('./package.json').version")
echo "https://img.shields.io/npm/v/YOUR_PACKAGE.svg"
```

#### Downloads Badge

```bash
echo "https://img.shields.io/npm/dt/YOUR_PACKAGE.svg"
```

#### License Badge

```bash
LICENSE=$(node -p "require('./package.json').license")
echo "https://img.shields.io/badge/License-$LICENSE-blue.svg"
```

#### Build Badge

```bash
echo "https://github.com/USER/REPO/actions/workflows/main.yml/badge.svg"
```

## Common Updates

### Update Version Badge

```markdown
# OLD
[![Version](https://img.shields.io/badge/v-1.0.0-blue)](https://www.npmjs.com/package/pkg)

# NEW
[![Version](https://img.shields.io/badge/v-2.0.0-blue)](https://www.npmjs.com/package/pkg)
```

### Add Missing Badge

```markdown
# ADD
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
```

### Fix Broken Badge

```markdown
# OLD (broken)
[![Build](https://badges.frapsoft.com/typescript/version](broken-url)](url)

# NEW (correct)
[![Build](https://github.com/user/repo/actions/workflows/main.yml/badge.svg)](url)
```

### Update Package Name

```markdown
# OLD
[![npm](https://img.shields.io/badge/npm-old-package)](url)

# NEW
[![npm](https://img.shields.io/badge/npm-new-package)](url)
```

## Badge Categories

### Package Badges

| Badge | URL Template |
|-------|--------------|
| Version | `https://img.shields.io/npm/v/{package}.svg` |
| Downloads | `https://img.shields.io/npm/dm/{package}.svg` |
| License | `https://img.shields.io/npm/l/{package}.svg` |

### GitHub Badges

| Badge | URL Template |
|-------|--------------|
| Build | `https://github.com/{user}/{repo}/actions/workflows/{workflow}/badge.svg` |
| Stars | `https://img.shields.io/github/stars/{user}/{repo}` |
| Forks | `https://img.shields.io/github/forks/{user}/{repo}` |
| Issues | `https://img.shields.io/github/issues/{user}/{repo}` |

### Code Quality

| Badge | Service |
|-------|---------|
| Coverage | Codecov, Coveralls |
| Lint | ESLint, Stylelint |
| Security | Snyk, CodeQL |

## Validation Checklist

- [ ] Version badge shows current version
- [ ] All links clickable
- [ ] Build status passing
- [ ] License correct
- [ ] No 404 badges
- [ ] All images load

## Fix Broken Badges

### Test All Badges

```bash
#!/bin/bash
# check-badges.sh

echo "Checking badges..."

while IFS= read -r line; do
  url=$(echo "$line" | grep -oP 'https://img\.shields\.io[^")]+')
  if [ -n "$url" ]; then
    status=$(curl -s -o /dev/null -w "%{http_code}" "$url")
    if [ "$status" = "404" ]; then
      echo "❌ BROKEN: $url"
    else
      echo "✅ OK: $url"
    fi
  fi
done < README.md
```

### Auto-Fix

```bash
# Update package name in all badges
sed -i 's/old-package/new-package/g' README.md

# Update version
sed -i 's/v1.0.0/v2.0.0/g' README.md

# Update repo
sed -i 's|github.com/olduser|github.com/newuser|g' README.md
```

## Badge Best Practices

### Recommended Badges

```markdown
<!-- Top of README -->
[![npm version](https://img.shields.io/npm/v/package.svg)](https://www.npmjs.com/package/package)
[![License](https://img.shields.io/npm/l/package.svg)](LICENSE)
[![Build](https://github.com/user/repo/actions/workflows/main.yml/badge.svg)](https://github.com/user/repo/actions)
```

### Avoid

- Too many badges (>5-7 at top)
- Outdated status badges
- Unnecessary badges
- Non-working links
