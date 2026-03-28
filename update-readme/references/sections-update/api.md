# Update API Section

How to keep API documentation synchronized with actual code.

## What to Verify

1. **All exported functions** - Documented
2. **Function signatures** - Match code
3. **Parameters** - Correct types and defaults
4. **Return types** - Accurate
5. **Deprecations** - Noted
6. **Examples** - Work

## Update Process

### Step 1: Extract Current API

#### JavaScript/TypeScript

```bash
# Get all exports
grep -rE "^export (function|const|class|interface|type)" src/ --include="*.ts" | \
  sed 's/.*export //' | sed 's/[{|(].*//' | sort -u

# Get function signatures
grep -rE "^export function \w+" src/ --include="*.ts" | \
  sed 's/.*export //' | sort -u
```

#### Python

```bash
# Get all public functions/classes
grep -E "^def |^class " module.py | grep -v "^_" | sed 's/:.*//'

# Get function signatures
grep -E "^def \w+\(" module.py | sed 's/:.*//'
```

#### Java

```bash
# Get public methods
grep -rE "public (void|String|int|boolean|List|Optional)" src/ --include="*.java" | \
  sed 's/.*public //' | sed 's/ {.*//' | sort -u
```

### Step 2: Compare with README

```bash
# Extract API from README
grep -oP "^### \`?\w+\(" README.md | sed 's/### `//' | sed 's/(//' | sort -u

# Compare
diff <(get_code_api) <(get_readme_api)
```

### Step 3: Generate API Docs

#### TypeScript with TypeDoc

```bash
# Install typedoc
npm install --save-dev typedoc

# Generate docs
npx typedoc src/index.ts --out docs/api

# Output markdown
npx typedoc src/index.ts --plugin typedoc-plugin-markdown
```

#### Python with Sphinx

```bash
# Install sphinx
pip install sphinx sphinx-autodoc

# Generate
sphinx-build -b markdown source docs
```

## Common Updates

### New Function

```markdown
### OLD

### myFunction(param)

Description...

---

### NEW

### myFunction(param)

Description...

### newFunction(options)

Description of new function...
```

### Changed Signature

```markdown
### OLD

### process(data, callback)

- `data` (string): Input data
- `callback` (function): Callback

---

### NEW

### process(options)

- `options` (object): Configuration object
  - `data` (string): Input data
  - `onComplete` (function): Callback
```

### Deprecated Function

```markdown
### OLD

### oldFunction()

Old function...

---

### NEW

### oldFunction()

> [!WARNING]
> **Deprecated** since v2.0. Use `newFunction()` instead.

Old function...
```

### Removed Function

```markdown
### REMOVED

> [!NOTE]
> The following were removed in v2.0:
> - `removedFunction()` - Use `replacementFunction()`
> - `oldMethod()` - No replacement
```

## API Documentation Template

### Function Documentation

```markdown
### functionName(param1, param2?)

Brief description of what this function does.

**Parameters:**

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `param1` | `string` | Yes | - | Description |
| `param2` | `boolean` | No | `false` | Optional param |

**Returns:** `Promise<string>`

**Throws:** `Error` - When something fails

**Example:**

```javascript
const result = await functionName('input', true);
console.log(result);
```
```

### Class Documentation

```markdown
### ClassName

Description of the class.

#### constructor(options?)

**Parameters:**

| Name | Type | Default | Description |
|------|------|---------|-------------|
| `options` | `object` | `{}` | Configuration |

#### methodName()

Description.

**Returns:** `void`
```

## Validation Checklist

- [ ] All exported functions documented
- [ ] Signatures match code
- [ ] Parameter types correct
- [ ] Return types accurate
- [ ] Examples work
- [ ] Deprecations noted
- [ ] Removed functions marked

## Auto-Generate API Docs

### TypeScript

Add to package.json:

```json
{
  "scripts": {
    "docs:api": "typedoc src/index.ts --out docs/api"
  }
}
```

### Python

```bash
# Sphinx conf.py
extensions = ['sphinx.ext.autodoc']
```

## Sync Workflow

1. **Before release:** Run API extraction
2. **Compare:** Diff current vs README
3. **Update:** Add new, mark deprecated
4. **Test:** Run all examples
5. **Commit:** Update README

## Tools

| Language | Tool |
|----------|------|
| TypeScript | TypeDoc |
| JavaScript | JSDoc + documentationjs |
| Python | Sphinx, pdoc |
| Java | Javadoc |
| Go | godoc |
