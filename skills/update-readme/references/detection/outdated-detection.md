# Outdated Detection

Detect common outdated patterns and obsolete information in README files.

## Common Outdated Patterns

### Version Numbers

**Problem:** Hardcoded old versions

```markdown
# BAD
npm install my-package@1.0.0
Requires Node.js 12+

# GOOD  
npm install my-package
Requires Node.js 18+
```

**Detection:**

```bash
# Find version-pinned installs
grep -E "(npm install|@)([0-9]+\.){2}[0-9]+" README.md

# Find outdated Node versions
grep -oP "Node\.js\s+(1[0-9]|20)" README.md || echo "Check Node versions"
```

### Deprecated APIs

**Problem:** Documentation for removed/deprecated features

```markdown
# BAD
### deprecatedFunction()

Use newFunction() instead.

# GOOD
### newFunction()
```

**Detection:**

```bash
# Find deprecated mentions
grep -i "deprecat" README.md

# Compare with code
grep -rE "@deprecated" src/ --include="*.js"
```

### Old File Paths

**Problem:** Referencing non-existent files

```markdown
# BAD
See `lib/old-file.js` for details.

# GOOD
See `src/utils/file.js` for details.
```

**Detection:**

```bash
# Extract file paths from README
grep -oP '(?<=`)[^`]+(?=`)' README.md | while read path; do
  [ -e "$path" ] || echo "❌ Missing: $path"
done
```

### Broken Links

**Problem:** URLs that no longer work

**Detection:**

```bash
# Check all links in README
grep -oP 'https?://[^)<>"]+' README.md | while read url; do
  status=$(curl -s -o /dev/null -w "%{http_code}" --max-time 5 "$url")
  if [ "$status" = "404" ] || [ "$status" = "000" ]; then
    echo "❌ Broken: $url (status: $status)"
  fi
done
```

### Old Requirements

**Problem:** Requirements lower than actual

```markdown
# BAD
Requires Python 3.8+

# GOOD
Requires Python 3.10+
```

**Detection:**

```bash
# Python - check setup.py / pyproject.toml
REQ_PYTHON=$(python -c "import tomllib; print(tomllib.load(open('pyproject.toml','rb'))['project']['requires-python'])" 2>/dev/null)

# Check README
README_PY=$(grep -oP 'Python\s+\K[0-9.]+' README.md | head -1)

echo "Required: $REQ_PYTHON, README: $README_PY"
```

### Outdated Commands

**Problem:** Old CLI commands

```markdown
# BAD
npm run build:prod

# GOOD
npm run build -- --mode production
```

**Detection:**

```bash
# Get current scripts
CURRENT_SCRIPTS=$(node -p "Object.keys(require('./package.json').scripts)" | tr ',' '\n')

# Check README commands
README_SCRIPTS=$(grep -oP 'npm run \K\w+' README.md)

# Find commands not in package.json
echo "$README_SCRIPTS" | while read script; do
  echo "$CURRENT_SCRIPTS" | grep -q "$script" || echo "❌ Not in package.json: $script"
done
```

## Pattern Detection Scripts

### Quick Scan

```bash
#!/bin/bash
# quick-outdated-scan.sh

echo "=== Quick Outdated Detection ==="

# 1. Version-pinned installs
echo -e "\n📦 Version-pinned installs:"
grep -nE "(@[0-9]+\.[0-9]+\.[0-9]+|v[0-9]+\.[0-9]+\.[0-9]+)" README.md | head -5

# 2. Old Node versions
echo -e "\n🟢 Old Node versions:"
grep -nE "Node\.js (1[0-4]|15|16|17)" README.md

# 3. Old Python versions  
echo -e "\n🐍 Old Python versions:"
grep -nE "Python (2\.|3\.[0-8])" README.md

# 4. Old package managers
echo -e "\n📦 Old package managers:"
grep -nE "(npm install -g|yarn add)" README.md | grep -v "yarn"

# 5. TODO/FIXME
echo -e "\n📝 TODO/FIXME in README:"
grep -nE "^.*TODO|^.*FIXME" README.md

# 6. Old links (sample)
echo -e "\n🔗 Checking sample links..."
curl -s -o /dev/null -w "%{http_code}" "https://nodejs.org" | grep -q "200" && echo "✅ nodejs.org works" || echo "❌ Check nodejs.org"

echo -e "\n=== Done ==="
```

### Full Scan

```bash
#!/bin/bash
# full-outdated-scan.sh

# Requires: npm, node, curl

echo "=== Full README Outdated Scan ==="

# Load package info
PKG_NAME=$(node -p "require('./package.json').name" 2>/dev/null)
PKG_VERSION=$(node -p "require('./package.json').version" 2>/dev/null)
PKG_NODE=$(node -p "require('./package.json').engines?.node" 2>/dev/null)

echo "Package: $PKG_NAME v$PKG_VERSION"
echo "Node requirement: $PKG_NODE"

# Check 1: Version in README
echo -e "\n[1] Version badge:"
grep -oP "v?$PKG_VERSION" README.md >/dev/null && echo "✅ Version current" || echo "❌ Version outdated"

# Check 2: Node requirement
echo -e "\n[2] Node.js requirement:"
if [ -n "$PKG_NODE" ]; then
  echo "$PKG_NODE"
fi

# Check 3: Deprecated APIs
echo -e "\n[3] Deprecated API mentions:"
grep -ci "deprecat" README.md && echo "⚠️ Found deprecated mentions" || echo "✅ No deprecated mentions"

# Check 4: Broken internal links
echo -e "\n[4] Internal file references:"
grep -oP '(?<=`)[^`]+(?=`)' README.md | grep -v "^http" | while read f; do
  [ -e "$f" ] || echo "❌ Missing: $f"
done

# Check 5: Badge URLs
echo -e "\n[5] Badge validation:"
grep -oP 'https://img\.shields\.io[^")]+' README.md | head -3 | while read url; do
  status=$(curl -s -o /dev/null -w "%{http_code}" "$url" 2>/dev/null)
  [ "$status" = "200" ] && echo "✅ $url" || echo "❌ $url (broken)"
done

# Check 6: Code examples syntax
echo -e "\n[6] Checking code block format:"
grep -c "^```" README.md | awk '{print "Code blocks: " $1}'

echo -e "\n=== Scan Complete ==="
```

## Warning Flags

Add these markers to flag sections needing review:

```markdown
> [!WARNING]
> ⚠️ This section may be outdated. Last verified: 2024-01

<!-- TODO: Update installation for v2.0 -->
<!-- FIXME: Fix broken link -->
```

## Auto-Comment on Outdated

GitHub Actions can add labels:

```yaml
name: Check README
on: [pull_request]

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run outdated detection
        run: |
          # Run detection scripts
          bash .github/scripts/readme-check.sh
          
          # Add comment if issues found
          if [ -f README_ISSUES.md ]; then
            gh pr comment $PR_NUMBER --body-file README_ISSUES.md
          fi
```
