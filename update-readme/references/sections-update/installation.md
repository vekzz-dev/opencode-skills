# Update Installation Section

How to keep the Installation section current and accurate.

## What to Verify

1. **Package name** - Matches `package.json` name
2. **Version** - Current or `latest`
3. **Package manager commands** - All work correctly
4. **Peer dependencies** - Mentioned if required
5. **Platform-specific** - Notes for Windows/Linux/Mac

## Update Process

### Step 1: Get Current Package Info

```bash
# Get package name
node -p "require('./package.json').name"

# Get current version
node -p "require('./package.json').version"

# Get package manager
cat package.json | grep -A5 '"publishConfig"'
```

### Step 2: Verify Commands Work

```bash
# Test npm install (dry run)
npm install --dry-run

# Test package exists on npm
npm view . name version

# Test pnpm if used
which pnpm && pnpm install --dry-run

# Test yarn if used  
which yarn && yarn install --dry-run
```

### Step 3: Check for Breaking Changes

```bash
# Check for peer dependencies
node -p "require('./package.json').peerDependencies"

# Check if any new peer deps
npm info . peerDependencies
```

## Common Updates

### npm Package

**Update version badge:**

```markdown
[![npm version](https://img.shields.io/npm/v/YOUR_PACKAGE)](https://www.npmjs.com/package/YOUR_PACKAGE)
```

**Update installation commands:**

```markdown
## Installation

```bash
npm install YOUR_PACKAGE
```
```bash  
yarn add YOUR_PACKAGE
```
```bash
pnpm add YOUR_PACKAGE
```
```

### Python Package

**Update with pip:**

```bash
# Get package name from pyproject.toml
python -c "import tomllib; print(tomllib.load(open('pyproject.toml'),'rb')['project']['name'])"
```

**Update README:**

```markdown
## Installation

```bash
pip install package-name
```

With extras:

```bash
pip install package-name[dev]
```
```

### Java/Maven

**Update Maven coordinates:**

```xml
<dependency>
  <groupId>com.example</groupId>
  <artifactId>artifact-name</artifactId>
  <version>VERSION</version>
</dependency>
```

## Version Handling

### Pin Exact Version

```markdown
npm install lodash@4.17.21
```

**When to use:** When you need reproducibility

### Use Latest

```markdown
npm install lodash
```

**When to use:** Default, recommended

### Use Semver Range

```markdown
npm install lodash@^4.17.0
```

**When to use:** When you accept minor updates

## Template Updates

### Node.js Library

```markdown
## Installation

```bash
# npm
npm install my-library

# yarn
yarn add my-library

# pnpm
pnpm add my-library
```
```

### Python Package

```markdown
## Installation

```bash
pip install my-package
```

From source:

```bash
git clone https://github.com/user/my-package.git
cd my-package
pip install -e .
```
```

### CLI Tool

```markdown
## Installation

```bash
# npm global
npm install -g my-cli

# Homebrew
brew install my-cli

# Cargo
cargo install my-cli
```
```

## Validation Checklist

- [ ] Package name matches exactly
- [ ] Version is current or uses latest
- [ ] All package managers tested
- [ ] Peer dependencies documented
- [ ] Platform notes accurate
- [ ] Commands copy-paste work

## Auto-Update Commands

Add to package.json scripts:

```json
{
  "scripts": {
    "readme:check-install": "npm view . name version"
  }
}
```

## Troubleshooting

### Package Not Found

```bash
# Check if published
npm view package-name

# Check spelling
npm search keyword
```

### Wrong Version Showing

```bash
# Clear npm cache
npm cache clean --force

# Reinstall
npm install
```
