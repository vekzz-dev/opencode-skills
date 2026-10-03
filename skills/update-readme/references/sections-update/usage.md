# Update Usage Section

How to keep code examples accurate and working.

## What to Verify

1. **All imports** - Exist in codebase
2. **All functions** - Still available
3. **Syntax** - Works in current version
4. **Output** - Matches expected results
5. **Configuration** - Still valid

## Update Process

### Step 1: Extract Code Examples

```bash
# Extract all JavaScript code blocks
grep -A5 "```javascript" README.md | head -50

# Extract Python code blocks
grep -A5 "```python" README.md | head -50

# Extract shell commands
grep -A3 "```bash" README.md | head -30
```

### Step 2: Verify Each Example

#### JavaScript/TypeScript

```bash
# Create test file
cat > /tmp/test-example.mjs << 'EOF'
// Paste example from README
import { func } from 'my-package';

const result = func('input');
console.log(result);
EOF

# Run test
node /tmp/test-example.mjs
```

#### Python

```bash
# Create test file
cat > /tmp/test_example.py << 'EOF'
# Paste example from README
from package import func

result = func('input')
print(result)
EOF

# Run test
python /tmp/test_example.py
```

### Step 3: Compare with Code

```bash
# Get actual exports
grep -rE "^export (function|const|class)" src/ --include="*.ts"

# Compare with README imports
grep -oP "import.*from" README.md
```

## Common Updates

### Import Changes

```markdown
# OLD (outdated)
import { oldName } from 'package';

# NEW (current)
import { newName } from 'package';
```

### API Changes

```markdown
# OLD
package.doSomething(arg1, arg2);

# NEW
package.doSomething({ arg1, arg2 });
```

### Configuration Changes

```markdown
# OLD
new Package({ option: value });

# NEW  
new Package({ option: value, newOption: true });
```

## Testing Examples

### Automated Testing

```bash
#!/bin/bash
# test-readme-examples.sh

# Test npm scripts
for script in $(grep -oP 'npm run \K\w+' README.md); do
  npm run $script --dry-run >/dev/null 2>&1
  [ $? -eq 0 ] && echo "✅ npm run $script" || echo "❌ npm run $script"
done
```

### Manual Verification

1. Copy code from README
2. Paste into new file
3. Run in clean environment
4. Verify output matches docs

## Example Types to Check

### Basic Usage

```javascript
// Must work out of the box
import pkg from 'package';

const result = pkg.main();
```

### Configuration

```javascript
// Must match actual options
const pkg = new Package({
  option1: true,
  option2: 'value'
});
```

### CLI Commands

```bash
# Must work as documented
my-cli command --flag value
```

### API Calls

```javascript
// Must match current API
const result = await api.endpoint({
  param: value
});
```

## Template Updates

### Simple Import

```markdown
## Usage

```javascript
import { myFunction } from 'my-package';

const result = myFunction('input');
console.log(result);
```
```

### With Configuration

```markdown
## Usage

```javascript
import MyPackage from 'my-package';

const pkg = new MyPackage({
  apiKey: process.env.API_KEY,
  debug: true
});

const result = pkg.process(data);
```
```

### CLI

```markdown
## Usage

```bash
# Initialize project
my-cli init

# Build
my-cli build --production

# Deploy
my-cli deploy staging
```
```

## Validation Checklist

- [ ] All imports exist
- [ ] All methods available
- [ ] Syntax is current
- [ ] Output matches docs
- [ ] Configuration options valid
- [ ] CLI commands work

## Auto-Test Examples

Add to CI:

```yaml
name: Test README Examples
on: [push, pull_request]

jobs:
  test-examples:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Test examples
        run: |
          npm install
          node -e "require('./examples/basic.js')"
```

## Common Issues

### Broken Import

```javascript
// Error: Cannot find module
// Fix: Update import path
import { func } from 'package';  // was: from './old-path'
```

### Missing Option

```javascript
// Error: Unknown option
// Fix: Update config
new Package({ option: true });  // was: { oldOption: true }
```

### Deprecated Method

```javascript
// Warning: Deprecated
// Fix: Use new method
pkg.newMethod();  // was: pkg.oldMethod()
```
