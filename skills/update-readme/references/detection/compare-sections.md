# Compare Sections

Compare each README section against the actual codebase to find discrepancies.

## Section-by-Section Comparison

### 1. Title & Badges

**What to check:**
- Project name matches `package.json` name
- Version badge shows current version
- Build badge points to correct branch
- License badge is correct

**How to verify:**

```bash
# Title
PKG_NAME=$(node -p "require('./package.json').name")
README_NAME=$(grep -oP '(?<=^# ).+' README.md | head -1)
[ "$PKG_NAME" = "$README_NAME" ] && echo "✅ Name matches" || echo "❌ Name mismatch"

# Version badge
PKG_VERSION=$(node -p "require('./package.json').version")
grep -q "v$PKG_VERSION" README.md && echo "✅ Version badge current" || echo "❌ Version badge outdated"
```

### 2. Installation

**What to check:**
- Package manager commands work
- Package name correct
- Version correct or uses `latest`
- All installation methods (npm, yarn, pnpm) if applicable

**How to verify:**

```bash
# Test npm install command
npm pack --dry-run 2>&1 | grep -q "packaging" && echo "✅ Package valid"

# Test pip install (Python)
pip index versions package-name >/dev/null 2>&1 && echo "✅ Package exists on PyPI"

# Check all mentioned tools
for tool in npm yarn pnpm; do
  which $tool >/dev/null && echo "✅ $tool available" || echo "⚠️ $tool not installed"
done
```

### 3. Requirements

**What to check:**
- Node.js/Python/Java version requirements
- These match `package.json` engines
- No outdated version numbers

**How to verify:**

```bash
# Node.js
PKG_NODE=$(node -p "require('./package.json').engines.node" 2>/dev/null)
echo "Package requires: $PKG_NODE"

# Extract from README
README_NODE=$(grep -oP 'Node\.js\s+\K[0-9.]+' README.md | head -1)
echo "README mentions: $README_NODE"

[ "$PKG_NODE" = "*" ] && echo "✅ Any Node.js" || [ "$PKG_NODE" = "$README_NODE" ] && echo "✅ Match" || echo "❌ Mismatch"
```

### 4. Usage / Examples

**What to check:**
- All imports exist in code
- Functions/methods exist
- Return values match
- No deprecated APIs

**How to verify:**

```bash
# Extract imports from README
grep -oP "import .* from" README.md | sed "s/import //" | sed "s/ from//" | sort -u > /tmp/readme_imports.txt

# Extract exports from code
grep -rE "^export " src/ --include="*.ts" | sed 's/.*export //' | sed 's/[({].*//' | sort -u > /tmp/code_exports.txt

# Compare
diff /tmp/readme_imports.txt /tmp/code_exports.txt || echo "⚠️ Differences found"
```

### 5. API Reference

**What to check:**
- All documented functions exist
- Parameters match
- Return types correct

**How to verify:**

```bash
# Get all exported functions from code
grep -rE "export (function|const|class) \w+" src/ | sed 's/.*export //' | sed 's/[({].*//' > /tmp/api_code.txt

# Get all documented in README
grep -oP "^### \`?\K[^\]`.*?(?=\()`" README.md > /tmp/api_readme.txt

# Find missing from README
comm -23 /tmp/api_code.txt /tmp/api_readme.txt
```

### 6. Configuration

**What to check:**
- ENV variables still valid
- Config file examples work
- Default values match code

**How to verify:**

```bash
# Extract ENV vars from code
grep -rE "process\.env\.\w+" src/ | grep -oE "process\.env\.\w+" | sort -u

# Extract from README
grep -oP '\$[A-Z_]+|[A-Z_]+=' README.md | sort -u
```

### 7. Scripts / Commands

**What to check:**
- All documented npm scripts exist
- Commands work as documented

**How to verify:**

```bash
# Get scripts from package.json
node -p "Object.keys(require('./package.json').scripts)" > /tmp/scripts_code.txt

# Get scripts from README
grep -oP 'npm run \K\w+' README.md > /tmp/scripts_readme.txt

# Find undocumented scripts
comm -23 /tmp/scripts_code.txt /tmp/scripts_readme.txt
```

### 8. Dependencies

**What to check:**
- Major dependencies mentioned
- No removed dependencies
- Version constraints accurate

**How to verify:**

```bash
# Get top dependencies
npm ls --depth=0 | head -20

# Check README mentions key deps
for dep in react express fastapi; do
  grep -q "$dep" README.md && echo "✅ $dep mentioned" || echo "⚠️ $dep not in README"
done
```

## Comparison Matrix

| Section | What to Compare | Tool |
|---------|----------------|------|
| Title | package.json name | diff |
| Badges | Version numbers | grep |
| Install | Package name, version | curl/test |
| Requirements | engines field | diff |
| Usage | Import statements | diff |
| API | Function signatures | diff |
| Config | ENV variables | grep |
| Scripts | package.json scripts | diff |

## Report Generation

```bash
# Generate outdated report
echo "# README Comparison Report" > README_REPORT.md
echo "Generated: $(date)" >> README_REPORT.md
echo "" >> README_REPORT.md

# Run all checks and append
echo "## Installation" >> README_REPORT.md
# (run checks) >> README_REPORT.md

echo "## Usage" >> README_REPORT.md
# (run checks) >> README_REPORT.md
```

## Action Items

After comparison, create action list:

1. **Critical** - Broken examples, wrong version
2. **Important** - Missing API docs, outdated requirements
3. **Nice-to-have** - Missing badges, better formatting

```markdown
## README Update Action Items

### Critical
- [ ] Fix npm install command (package renamed)
- [ ] Update API docs (new method added)

### Important
- [ ] Add Python 3.12 to requirements
- [ ] Update Node.js to 20+

### Nice-to-have
- [ ] Add codecov badge
- [ ] Add code coverage section
```
