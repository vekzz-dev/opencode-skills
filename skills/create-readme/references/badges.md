# Badges Reference

Use badges to provide quick information about your project at a glance.

## Badge Services

### Shields.io (Recommended)

Shields.io is the most popular badge service, free and customizable.

**Base URL:** `https://img.shields.io`

**Format:**
```markdown
[![Label](https://img.shields.io/badge/<LABEL>-<MESSAGE>-<COLOR>?style=<STYLE>)](URL)
```

### npm Badges

```markdown
[![npm version][npm-badge]][npm-url]
[![npm downloads][npm-dl-badge]][npm-url]
```

```markdown
[npm-badge]: https://img.shields.io/npm/v/package-name
[npm-dl-badge]: https://img.shields.io/npm/dt/package-name
[npm-url]: https://www.npmjs.com/package/package-name
```

### PyPI Badges

```markdown
[![PyPI version][pypi-badge]][pypi-url]
[![Python Versions][python-badge]][pypi-url]
[![PyPI downloads][pypi-dl-badge]][pypi-url]
```

```markdown
[pypi-badge]: https://img.shields.io/pypi/v/package-name
[pypi-url]: https://pypi.org/project/package-name/
[pypi-dl-badge]: https://img.shields.io/pypi/dm/package-name
[python-badge]: https://img.shields.io/pypi/pyversions/package-name
```

### Maven Central Badges

```markdown
[![Maven Central](https://img.shields.io/maven-central/v/com.example/package)](https://central.sonatype.com/namespace/com.example)
```

### GitHub Badges

```markdown
[![License][license-badge]][license-url]
[![Build Status][build-badge]][build-url]
[![Last Commit][commit-badge]][commit-url]
[![Contributors][contributors-badge]][contributors-url]
```

```markdown
[license-badge]: https://img.shields.io/github/license/username/repo
[license-url]: https://github.com/username/repo/blob/main/LICENSE
[build-badge]: https://github.com/username/repo/actions/workflows/main.yml/badge.svg
[build-url]: https://github.com/username/repo/actions
[commit-badge]: https://img.shields.io/github/last-commit/username/repo
[commit-url]: https://github.com/username/repo/commits/main
[contributors-badge]: https://img.shields.io/github/contributors/username/repo
[contributors-url]: https://github.com/username/repo/graphs/contributors
```

## Color Reference

| Color Name | Hex | Use Case |
|------------|-----|----------|
| brightgreen | 4CB110 | Success, stable |
| green | 97CA00 | Good, ready |
| yellowgreen | YAC15C | Warning, beta |
| yellow | FABC0C | Caution |
| orange | FE8D06 | Warning |
| red | E74C3C | Error, broken |
| blue | 0078D4 | Info, link |
| purple | 9B59B6 | Special |
| grey | 738194 | Neutral |

## Styles

| Style | Example |
|-------|--------|
| flat (default) | ![](https://img.shields.io/badge/test-OK-green) |
| flat-square | ![](https://img.shields.io/badge/test-OK-green?style=flat-square) |
| plastic | ![](https://img.shields.io/badge/test-OK-green?style=plastic) |
| for-the-badge | ![](https://img.shields.io/badge/test-OK-green?style=for-the-badge) |
| social | ![](https://img.shields.io/badge/Owner-Name-blue?style=social) |

## Common Badge Patterns

### Version Badge

```markdown
[![npm version](https://img.shields.io/npm/v/package-name)](https://www.npmjs.com/package/package-name)
```

### Download Badge

```markdown
[![npm downloads](https://img.shields.io/npm/dt/package-name)](https://www.npmjs.com/package/package-name)
```

### License Badge

```markdown
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
```

### Build Status Badge

```markdown
[![Build Status](https://github.com/username/repo/actions/workflows/main.yml/badge.svg)](https://github.com/username/repo/actions)
```

### Code Coverage Badge

```markdown
[![Coverage](https://img.shields.io/codecov/c/github/username/repo)](https://app.codecov.io/gh/username/repo)
```

### Language Stats Badge

```markdown
[![Languages](https://img.shields.io/github/languages/count/username/repo)](https://github.com/username/repo)
```

### Bundle Size Badge

```markdown
[![Bundle Size](https://img.shields.io/bundlephobia/min/package-name)](https://bundlephobia.com/package/package-name)
```

### Dependency Count Badge

```markdown
[![Dependencies](https://img.shields.io/librariesio/dependent-repos/npm/package-name)](https://libraries.io/npm/package-name)
```

## Language-Specific Badges

### Node.js

```markdown
[![Node](https://img.shields.io/node/v/package-name)](https://nodejs.org)
```

### Python

```markdown
[![Python](https://img.shields.io/pypi/pyversions/package-name)](https://www.python.org)
```

### Java

```markdown
[![Java](https://img.shields.io/badge/Java-17+-blue?logo=openjdk)](https://www.java.com)
```

### Go

```markdown
[![Go](https://img.shields.io/badge/Go-1.20+-blue?logo=go)](https://go.dev)
```

### Rust

```markdown
[![Rust](https://img.shields.io/badge/Rust-1.70+-blue?logo=rust)](https://www.rust-lang.org)
```

## Social Badges

### Sponsor

```markdown
[![Sponsor](https://img.shields.io/badge/sponsor-%E2%9D%A4-red?logo=github)](https://github.com/sponsors/username)
```

### Follow

```markdown
[![Follow](https://img.shields.io/badge/Follow-@username-blue?logo=twitter)](https://twitter.com/username)
```

## Placement

### Top of README

```markdown
# Project Name

[![Badge1](url)] [![Badge2](url)]

Description...
```

### Above Installation

```markdown
## Installation

[![npm](https://img.shields.io/npm/v/package-name)](https://www.npmjs.com)
```

## Dynamic Badges

Shields.io supports dynamic badges that fetch data:

```markdown
![Docker Pulls](https://img.shields.io/docker/pulls/username/image)
![GitHub Issues](https://img.shields.io/github/issues-raw/username/repo)
![GitHub Stars](https://img.shields.io/github/stars/username/repo)
```

## Badge Links

Always link badges to relevant pages:

```markdown
[![npm version](https://img.shields.io/npm/v/package-name)](https://www.npmjs.com/package/package-name)
  ^ Link to npm page
```

## Tips

1. **Don't overdo it** - 3-5 badges max at top
2. **Link everything** - Make badges clickable
3. **Consistent style** - Use same style across badges
4. **Meaningful badges** - Include what matters
5. **Keep updated** - Remove outdated badges
