# Detect Changes

How to detect what in your README is outdated by comparing with the actual codebase.

## Compare Package.json vs README

### Installation Commands

```bash
# Get actual package name
PACKAGE_NAME=$(node -p "require('./package.json').name")

# Get actual version
PACKAGE_VERSION=$(node -p "require('./package.json').version")

# Get Node.js requirement
NODE_VERSION=$(node -p "require('./package.json').engines.node")

# Check README mentions correct version
grep -E "Node\.js.*$NODE_VERSION" README.md || echo "OUTDATED: Node.js version"
```

### Python Projects

```bash
# Get package name from pyproject.toml
PYTHON_NAME=$(python -c "import tomllib; print(tomllib.load(open('pyproject.toml','rb'))['project']['name'])")

# Get Python version requirement
PYTHON_REQ=$(python -c "import tomllib; print(tomllib.load(open('pyproject.toml','rb'))['project']['requires-python'])")

# Check README
grep -E "$PYTHON_REQ" README.md || echo "OUTDATED: Python version"
```

## Compare Code vs API Docs

### JavaScript/TypeScript

```bash
# List all exported functions
grep -E "^export (function|const|class)" src/index.ts

# Compare with README API section
echo "Functions in code:" && grep -E "^export" src/index.ts
echo "Functions in README:" && grep -E "^### .*\(" README.md
```

### Python

```bash
# List all public functions
grep -E "^def |^class " module.py | grep -v "^_"

# Compare with README
grep -E "def .*\(" README.md
```

### Java

### Go

```bash
# Get module name and version from go.mod
GO_MODULE=$(grep "^module " go.mod | awk '{print $2}')
GO_VERSION=$(grep "^go " go.mod | awk '{print $2}')

# Check README
grep -q "$GO_MODULE" README.md && echo "✅ Module mentioned" || echo "❌ Module not in README"
grep -q "go $GO_VERSION" README.md || echo "⚠️ Check Go version"
```

### Rust

```bash
# Get package name and version from Cargo.toml
RUST_NAME=$(grep "^name = " Cargo.toml | tr -d '"' | head -1)
RUST_VERSION=$(grep "^version = " Cargo.toml | tr -d '"' | head -1)

# Check README
grep -q "$RUST_NAME" README.md && echo "✅ Package mentioned" || echo "❌ Package not in README"
```

### Ruby

```bash
# Get gem name from gemspec
RUBY_NAME=$(grep "^spec.name" *.gemspec | awk '{print $3}' | tr -d "'")
RUBY_VERSION=$(grep "^spec.version" *.gemspec | awk '{print $3}' | tr -d "'")

# Check README
grep -q "$RUBY_NAME" README.md && echo "✅ Gem mentioned" || echo "❌ Gem not in README"
```

### .NET

```bash
# Get package name from csproj
DOTNET_NAME=$(grep "<RootNamespace>" *.csproj | head -1 | sed 's/.*>//;s/<.*//')
DOTNET_VERSION=$(grep "<Version>" *.csproj | head -1 | sed 's/.*>//;s/<.*//')

# Check README
grep -q "$DOTNET_NAME" README.md && echo "✅ Package mentioned" || echo "❌ Package not in README"
```

### Docker

```bash
# Check Dockerfile exists
[ -f Dockerfile ] && echo "✅ Dockerfile exists" || echo "❌ No Dockerfile"

# Check docker-compose
[ -f docker-compose.yml ] && echo "✅ docker-compose.yml exists" || echo "⚠️ No docker-compose"

# Get base image
BASE_IMAGE=$(grep "^FROM " Dockerfile | head -1 | awk '{print $2}')
grep -q "$BASE_IMAGE" README.md || echo "⚠️ Base image not in README"
```

```bash
# List public methods
grep -E "public (void|String|int|boolean|List)" src/main/java/pkg/*.java

# Check README
grep -E "### .*(" README.md
```

## Detect Breaking Changes

### Package.json Scripts

```bash
# Get current scripts
node -p "JSON.stringify(require('./package.json').scripts, null, 2)"

# Compare with README
grep -E "npm run" README.md
```

### Import Statements

```bash
# Get all imports
grep -r "^import " src/ --include="*.ts" | cut -d: -f2 | sort -u

# Check README
grep -E "import .*from" README.md
```

## Diff Detection Workflow

### Step 1: Generate Current State

```bash
# Create temp file with current exports
node -e "
const pkg = require('./package.json');
console.log('Name:', pkg.name);
console.log('Version:', pkg.version);
console.log('Main:', pkg.main);
console.log('Exports:', Object.keys(pkg.exports || {}));
"
```

### Step 2: Compare with README

Compare output with README sections:

1. **Installation** - Package name and version
2. **Usage** - Imports and exports
3. **API** - Functions and classes
4. **Requirements** - Node/version/engine

### Step 3: Flag Differences

Mark outdated sections:

```markdown
## Installation

> [!WARNING]
> ⚠️ Update needed: Package version changed from 1.0.0 to 2.0.0
```

## Automated Detection Script

```bash
#!/bin/bash
# detect-outdated.sh

echo "=== README Outdated Detection ==="

# Check 1: Version mismatch
README_VERSION=$(grep -oP '(?<=v)\d+\.\d+\.\d+' README.md | head -1)
PKG_VERSION=$(node -p "require('./package.json').version")

if [ "$README_VERSION" != "$PKG_VERSION" ]; then
  echo "❌ Version outdated: README v$README_VERSION vs package $PKG_VERSION"
else
  echo "✅ Version current"
fi

# Check 2: Node.js requirement
PKG_NODE=$(node -p "require('./package.json').engines.node" 2>/dev/null)
if [ -n "$PKG_NODE" ]; then
  if grep -q "Node\.js.*$PKG_NODE" README.md; then
    echo "✅ Node.js requirement current"
  else
    echo "❌ Node.js requirement outdated: $PKG_NODE"
  fi
fi

# Check 3: Broken badges
echo "Checking badges..."
grep -oP 'https://img\.shields\.io[^")]+' README.md | while read url; do
  status=$(curl -s -o /dev/null -w "%{http_code}" "$url")
  if [ "$status" != "200" ]; then
    echo "❌ Broken badge: $url"
  fi
done

# Check 4: Commands exist
echo "Checking npm scripts..."
node -p "Object.keys(require('./package.json').scripts)" | tr ',' '\n' | while read script; do
  if ! grep -q "npm run $script" README.md; then
    echo "⚠️ Missing script in README: $script"
  fi
done

echo "=== Done ==="
```

## Git Comparison

### Track README Changes

```bash
# See recent README changes
git log --oneline README.md -10

# See what changed in last commit
git diff HEAD~1 README.md
```

### Compare with Tags

```bash
# Compare README at different versions
git diff v1.0.0..v2.0.0 README.md
```
