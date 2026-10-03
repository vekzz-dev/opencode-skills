# Common README Issues

Solutions for frequently encountered README problems.

## Broken Examples

### Problem: Code Doesn't Run

```bash
# Test code in README
cat > /tmp/test.js << 'EOF'
// Paste from README
EOF
node /tmp/test.js
```

**Solution:** Always test code before committing.

### Problem: Wrong Import Path

```markdown
# WRONG
import { func } from './old-path';

# CORRECT  
import { func } from 'package-name';
```

### Problem: Missing Dependencies

```markdown
# WRONG
const result = require('external-package');

# CORRECT
# First install: npm install external-package
const result = require('external-package');
```

## Version Issues

### Problem: Wrong Version Badge

```bash
# Check current version
npm view . version

# Update badge
sed -i 's/v1.0.0/v2.0.0/g' README.md
```

### Problem: Outdated Requirements

```markdown
# WRONG
Requires Node.js 12

# CORRECT
Requires Node.js 18+ (LTS)
```

### Problem: Old Package Name

```markdown
# WRONG
npm install old-package

# CORRECT
npm install new-package
```

## Link Problems

### Problem: Broken Links

```bash
# Check all links
grep -oP 'https?://[^)<>"]+' README.md | while read url; do
  status=$(curl -s -o /dev/null -w "%{http_code}" --max-time 5 "$url")
  [ "$status" = "404" ] && echo "❌ $url"
done
```

**Solution:** Update or remove broken links.

### Problem: Relative Paths

```markdown
# WRONG
[See docs](docs/guide.md)

# CORRECT
[See docs](docs/guide.md)  # If file exists
[External](https://example.com)  # If external
```

### Problem: Anchor Links

```markdown
# WRONG
[See installation](#installation)

# CORRECT
[See installation](#installation)  # Ensure heading exists
```

## Badge Problems

### Problem: Missing Badge

```markdown
# ADD
[![npm](https://img.shields.io/npm/v/package)](https://npmjs.com/package/package)
```

### Problem: Wrong Package Name

```markdown
# WRONG
[![npm](https://img.shields.io/npm/v/wrong-package)]

# CORRECT
[![npm](https://img.shields.io/npm/v/correct-package)]
```

### Problem: Broken Badge

```bash
# Fix: Update shield URL
sed -i 's|github.com/wrong|github.com/correct|g' README.md
```

## Formatting Issues

### Problem: Unclosed Code Block

```markdown
# FIX
\`\`\`
code here
\`\`\`
```

### Problem: Table Not Rendering

```markdown
# FIX - Add proper spacing
| Header 1 | Header 2 |
|----------|----------|
| Cell 1   | Cell 2   |
```

### Problem: Images Not Loading

```markdown
# FIX - Check path
![Screenshot](assets/screenshot.png)  # File must exist
```

## Content Issues

### Problem: Missing Sections

```markdown
# ADD if missing
## Installation
## Usage  
## License
```

### Problem: Outdated Information

```markdown
# MARK as outdated
> [!WARNING]
> This feature was deprecated in v2.0. Use `newMethod()` instead.
```

### Problem: Too Long

```markdown
# FIX - Move to separate docs
## See Also

- [Full Documentation](docs/)
- [API Reference](docs/api.md)
- [Examples](examples/)
```

## Automation Issues

### Problem: Workflow Not Running

```yaml
# FIX - Check trigger
on:
  push:
    branches: [main]  # Must be correct branch
  workflow_dispatch:   # Enable manual run
```

### Problem: Token Permissions

```yaml
# FIX - Add permissions
permissions:
  contents: write
```

## Quick Fixes

### Fix All Versions

```bash
# Update all version numbers
VERSION=$(node -p "require('./package.json').version")
sed -i "s/v[0-9]\+\.[0-9]\+\.[0-9]\+/v$VERSION/g" README.md
```

### Fix All Links

```bash
# Replace old domain
sed -i 's|old-domain.com|new-domain.com|g' README.md
```

### Fix All Badges

```bash
# Update package name in badges
sed -i 's|old-package|new-package|g' README.md
```

## Validation Checklist

Before committing:

- [ ] All examples tested
- [ ] All links work
- [ ] All badges render
- [ ] All versions current
- [ ] All sections present

## Emergency Rollback

```bash
# If something breaks
git checkout HEAD -- README.md

# Or specific version
git checkout v1.0.0 -- README.md
```
