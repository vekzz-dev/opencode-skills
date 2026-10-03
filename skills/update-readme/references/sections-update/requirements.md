# Update Requirements Section

How to keep system and software requirements accurate.

## What to Verify

1. **Runtime versions** - Node.js, Python, Java, etc.
2. **Package managers** - npm, pip, maven versions
3. **Operating systems** - Windows, macOS, Linux
4. **Hardware requirements** - RAM, disk space
5. **External services** - Databases, APIs

## Update Process

### Step 1: Get Current Requirements

#### Node.js

```bash
# From package.json
node -p "require('./package.json').engines"

# Example output: { node: '^18.0.0', npm: '^9.0.0' }
```

#### Python

```bash
# From pyproject.toml
python -c "import tomllib; print(tomllib.load(open('pyproject.toml','rb'))['project']['requires-python'])"

# From setup.py
grep -E "python_requires" setup.py
```

#### Java

```bash
# From pom.xml
grep -A2 "<java.version>" pom.xml

# From build.gradle
grep "sourceCompatibility" build.gradle
```

### Step 2: Compare with README

```bash
# Extract from README
grep -oP "(Node\.js|Python|Java|Runtime)\s+\K[0-9.]+" README.md

# Compare with package.json
node -p "require('./package.json').engines.node"
```

### Step 3: Update

```markdown
## Requirements

- Node.js 18+
- npm 9+

---

# or

## Requirements

- Python 3.9+
- pip 21.0+

---

# or

## Requirements

- Java 17+
- Maven 3.8+
```

## Common Updates

### Node.js Version Bump

```markdown
# OLD
Requires Node.js 16+

# NEW
Requires Node.js 18+
```

### Python Version Update

```markdown
# OLD
Python 3.8+

# NEW
Python 3.10+
```

### Java Version Update

```markdown
# OLD
Java 11+

# NEW
Java 17+
```

## Requirements by Technology

### Node.js

```markdown
## Requirements

- Node.js 18.x or higher
- npm 9.x or higher

### Supported Platforms

- macOS 12+
- Windows 10+
- Linux (Ubuntu 20.04+, CentOS 8+)
```

### Python

```markdown
## Requirements

- Python 3.9 - 3.12
- pip 21.0+

### Supported Platforms

- macOS 12+
- Windows 10+
- Linux (Ubuntu 20.04+)
```

### Java

```markdown
## Requirements

- Java Development Kit (JDK) 17+
- Maven 3.8+ or Gradle 8+

### Supported Platforms

- macOS (Apple Silicon + Intel)
- Windows 10+
- Linux (Ubuntu 20.04+)
```

## Validation Checklist

- [ ] Runtime version current
- [ ] Package manager version correct
- [ ] OS requirements accurate
- [ ] Architecture noted (arm64, x64)
- [ ] External dependencies documented

## Version Detection Scripts

```bash
#!/bin/bash
# check-requirements.sh

echo "=== Requirements Check ==="

# Node.js
PKG_NODE=$(node -p "require('./package.json').engines?.node" 2>/dev/null)
README_NODE=$(grep -oP 'Node\.js\s+\K[0-9.]+' README.md | head -1)
echo "Package requires: $PKG_NODE"
echo "README mentions: $README_NODE"

# Python
if [ -f "pyproject.toml" ]; then
  REQ_PY=$(python -c "import tomllib; print(tomllib.load(open('pyproject.toml','rb'))['project'].get('requires-python','N/A'))" 2>/dev/null)
  echo "Python requires: $REQ_PY"
fi
```

## Auto-Update Requirements

Add to CI:

```yaml
name: Check Requirements
on: [pull_request]

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - name: Check Node version
        run: |
          PKG_NODE=$(node -p "require('./package.json').engines.node")
          README_NODE=$(grep -oP 'Node\.js\s+\K[0-9.]+' README.md)
          echo "package.json: $PKG_NODE"
          echo "README: $README_NODE"
```

## Common Issues

### Outdated Node Version

```markdown
# BAD
Requires Node.js 12+

# GOOD
Requires Node.js 18+ (LTS)
```

### Missing Package Manager

```markdown
# BAD
Requires Node.js

# GOOD
Requires Node.js 18+ and npm 9+
```

### Incomplete Requirements

```markdown
# BAD
Requirements: Node.js

# GOOD
Requirements:
- Node.js 18.x or higher
- npm 9.x or higher
- Git
```
