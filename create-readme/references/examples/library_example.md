# Example: npm Library README

This is an example of a well-structured library README.

```markdown
# date-fns

date-fns provides the most comprehensive, yet simple and consistent toolset for manipulating JavaScript dates in a browser & Node.js.

[![npm version][npm-badge]][npm-url]
[![License][license-badge]][license-url]
[![Build Status][build-badge]][build-url]

> date-fns is a lightweight (2KB gzipped) date manipulation library, 
> without dependencies.

## Why date-fns?

- **Lightweight** - 2KB gzipped
- **Immutable** - All functions return new dates
- **Pure functions** - No side effects
- **TypeScript** - Full TypeScript support
- **Universal** - Works in Node.js and browsers

## Installation

```bash
npm install date-fns
```

```bash
yarn add date-fns
```

```bash
pnpm add date-fns
```

## Usage

### JavaScript

```javascript
import { format, parseISO, addDays } from 'date-fns';

// Format a date
format(new Date(), 'MMM dd, yyyy') // 'Mar 24, 2026'

// Parse ISO string
parseISO('2026-03-24') // Date instance

// Add days
addDays(new Date(), 7) // Date instance + 7 days
```

### TypeScript

```typescript
import { format, FormatOptions } from 'date-fns';

const options: FormatOptions = {
  locale: es
};

format(new Date(), 'PPpp', options)
```

## API

### format

Format a date.

```javascript
format(date, formatStr, options?)
```

| Parameter | Type | Description |
|-----------|------|-------------|
| `date` | `Date \| number \| string` | Input date |
| `formatStr` | `string` | Format string |
| `options` | `object` | Options |

**Format tokens:**

| Token | Example | Description |
|-------|---------|-------------|
| `yyyy` | 2026 | 4-digit year |
| `MM` | 03 | 2-digit month |
| `dd` | 24 | 2-digit day |
| `HH` | 14 | 24-hour |
| `mm` | 30 | Minutes |
| `ss` | 00 | Seconds |

**Example:**

```javascript
format(new Date(), 'yyyy-MM-dd HH:mm:ss')
// '2026-03-24 14:30:00'
```

### addDays

Add days to a date.

```javascript
addDays(date, amount)
```

| Parameter | Type | Description |
|-----------|------|-------------|
| `date` | `Date \| number` | Input date |
| `amount` | `number` | Days to add |

**Returns:** `Date`

**Example:**

```javascript
addDays(new Date(2026, 2, 24), 7)
// Date instance: 2026-03-31
```

### isValid

Check if date is valid.

```javascript
isValid(date)
```

**Example:**

```javascript
isValid(new Date()) // true
isValid(new Date('invalid')) // false
```

## Locales

date-fns includes locales:

```javascript
import { format } from 'date-fns'
import { es, fr, de } from 'date-fns/locale'

format(new Date(), 'PPPP', { locale: es })
// 'martes, 24 de marzo de 2026'
```

## Time Zones

```javascript
import { format } from 'date-fns-tz'

format(now, 'yyyy-MM-dd HH:mm:ssXXX', { 
  timeZone: 'America/New_York' 
})
// '2026-03-24 10:30:00-04:00'
```

## Compatibility

| Environment | Support |
|-------------|---------|
| Node.js | 14+ |
| Browsers | Modern |
| Deno | ✅ |
| Bun | ✅ |

## Migration

### From Moment.js

```javascript
// Moment.js
moment(date).format('YYYY-MM-DD')

// date-fns
format(new Date(date), 'yyyy-MM-dd')
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

1. Fork the repo
2. Create your branch
3. Make changes
4. Submit PR

## License

MIT - see [LICENSE](LICENSE)

---

[npm-badge]: https://img.shields.io/npm/v/date-fns
[npm-url]: https://www.npmjs.com/package/date-fns
[license-badge]: https://img.shields.io/npm/l/date-fns
[build-badge]: https://github.com/date-fns/date-fns/actions/workflows/master.yml/badge.svg
```

## Key Takeaways

1. **One-liner** - Clear value proposition
2. **Why section** - Key benefits as bullet points
3. **Working code** - Real, runnable examples
4. **API tables** - Clear parameter documentation
5. **Code examples** - Before/after from alternatives
6. **Compatibility matrix** - Supported environments
