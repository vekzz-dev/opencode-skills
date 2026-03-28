# Library README Template

Use this template for npm, pip, Maven, or other package manager libraries.

## Structure

```markdown
# <library-name>

One-line description of what this library does.

[![npm version][npm-badge]][npm-url] 
[![License][license-badge]][license-url]

## Why?

Explain the problem this library solves. Why should someone use it?

## Installation

```bash
# npm
npm install <library-name>

# pnpm
pnpm add <library-name>

# yarn
yarn add <library-name>

# pip
pip install <library-name>

# Maven
<dependency>
  <groupId>com.example</groupId>
  <artifactId>library-name</artifactId>
  <version>1.0.0</version>
</dependency>
```

## Usage

Show the most common use case - the 80% that covers most scenarios.

```javascript
// JavaScript/TypeScript
import { mainFeature } from '<library-name>';

const result = mainFeature('input');
console.log(result);
```

```python
# Python
from library_name import main_feature

result = main_feature('input')
print(result)
```

```java
// Java
import com.example.Library;

var result = Library.mainFeature("input");
System.out.println(result);
```

## API

### mainFeature(input, options?)

Main exported function.

**Parameters:**

| Name | Type | Default | Description |
|------|------|---------|-------------|
| `input` | `string` | required | Input description |
| `options` | `object` | `{}` | Options object |

**Options:**

| Name | Type | Default | Description |
|------|------|---------|-------------|
| `option1` | `boolean` | `false` | Enable feature X |
| `option2` | `number` | `10` | Timeout in seconds |

**Returns:** `Promise<string>`

**Example:**

```javascript
const result = await mainFeature('test', { option1: true });
```

### ClassName

Another exported class.

#### constructor(options?)

**Parameters:**

| Name | Type | Default | Description |
|------|------|---------|-------------|
| `options` | `object` | `{}` | Configuration |

#### methodName()

Description of method.

**Returns:** `void`

## TypeScript

If TypeScript is supported, include types:

```typescript
import { mainFeature, MainFeatureOptions } from '<library-name>';

const options: MainFeatureOptions = {
  option1: true,
  option2: 300
};

const result = await mainFeature('input', options);
```

## Configuration

### Environment Variables

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `LIB_NAME_DEBUG` | `boolean` | `false` | Enable debug logging |

## Migration Guide

If migrating from v1 to v2 or from another library:

```javascript
// Before (old-library)
import { oldFunc } from 'old-library';

// After (this library)
import { mainFeature } from 'library-name';
```

## Comparison

Brief comparison with alternatives:

| Feature | This Library | Alternative |
|---------|-------------|------------|
| Feature A | ✅ | ✅ |
| Feature B | ✅ | ❌ |
| Size | 5KB | 50KB |

## Requirements

- Node.js 18+
- TypeScript 5.0+ (if using types)

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Short guide:

1. Fork the repository
2. Create a feature branch
3. Add tests
4. Submit a PR

## License

MIT - see [LICENSE](LICENSE)

## Acknowledgments

- Inspiration from [project-name](https://github.com/example)
- Thanks to [contributors](https://github.com/user/repo/graphs/contributors)
```

## Key Elements

1. **Why?** - Problem/solution statement
2. **Installation** - Package manager commands
3. **Usage** - Working code example
4. **API** - Table of functions/classes
5. **TypeScript** - If applicable
6. **Configuration** - ENV vars, options
7. **Migration** - If coming from another lib
8. **Comparison** - Why this vs alternatives
