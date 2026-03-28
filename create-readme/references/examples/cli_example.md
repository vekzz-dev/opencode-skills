# Example: CLI Tool README

This is an example of a well-structured CLI tool README.

```markdown
# create-next-app

Create Next.js apps with one command.

[![npm version][npm-badge]][npm-url]
[![License][license-badge]][license-url]
[![Build Status][build-badge]][build-url]

> The recommended way to create new Next.js apps.

## Why create-next-app?

- **Zero-config** - Works out of the box
- **Optimized** - Defaults to best practices
- **TypeScript** - Built-in TypeScript support
- **Tailwind** - Optional CSS support

## Installation

```bash
npm create next-app@latest
```

```bash
npx create-next-app@latest
```

```bash
yarn create next-app
```

```bash
pnpm create next-app
```

## Quick Start

```bash
# Interactive mode
npm create next-app@latest

# Flags
npx create-next-app@latest my-app --typescript --tailwind --eslint
```

## Commands

### create-next-app

Create a new Next.js application.

```bash
npx create-next-app@latest [project-name] [options]
```

**Arguments:**

| Argument | Description | Default |
|----------|-------------|---------|
| `project-name` | Name of the project | `my-next-app` |

**Options:**

| Flag | Alias | Default | Description |
|------|-------|---------|-------------|
| `--typescript` | `-ts` | `false` | Use TypeScript |
| `--javascript` | `-js` | `false` | Use JavaScript |
| `--tailwind` | `-t` | `false` | Use Tailwind CSS |
| `--eslint` | `-e` | `false` | Add ESLint |
| `--app` | | `true` | Use App Router |
| `--src-dir` | `-s` | `false` | Use `src/` directory |
| `--import-alias` | `-i` | `@/*` | Import alias |
| `--use-npm` | | | Force npm |
| `--use-yarn` | | | Force yarn |
| `--use-pnpm` | | | Force pnpm |
| `--example` | | | Example to use |

**Examples:**

```bash
# TypeScript + Tailwind
npx create-next-app@latest my-app --typescript --tailwind

# With specific example
npx create-next-app@latest my-app --example https://github.com/example/nextjs-example

# No installation, just files
npx create-next-app@latest my-app --no-install
```

## Interactive Mode

```bash
$ npx create-next-app@latest

√ What is your project named? ... my-app
√ Would you like to use TypeScript? ... No / Yes
√ Would you like to use ESLint? ... No / Yes
√ Would you like to use Tailwind CSS? ... No / Yes
√ Would you like to use `src/` directory? ... No / Yes
√ Would you like to use App Router? ... No / Yes
√ Would you like to customize the default import alias? ... No / Yes
√ Import alias ... @/*

Creating a new Next.js app in the current directory.
Installing dependencies...
```

## Configuration

### Environment

| Variable | Description |
|----------|-------------|
| `CI=1` | Run in non-interactive mode |
| `CREATE_NEXT_APP_TELEMETRY=0` | Disable telemetry |

### create-next-app.config.js

```javascript
module.exports = {
  typescript: true,
  tailwind: true,
  eslint: true,
}
```

## Project Structure

Created files:

```
my-app/
├── src/
│   ├── app/
│   │   ├── layout.tsx
│   │   ├── page.tsx
│   │   └── globals.css
│   └── lib/
├── public/
├── package.json
├── next.config.js
├── tailwind.config.js
└── tsconfig.json
```

## Next Steps

After creation:

```bash
cd my-app
npm run dev
```

Open [http://localhost:3000](http://localhost:3000)

## Troubleshooting

### "command not found: create-next-app"

Use npx:

```bash
npx create-next-app my-app
```

### Permission denied

```bash
sudo npx create-next-app my-app
# or
npm install -g create-next-app
```

## Compare

| Feature | create-next-app | Manual Setup |
|---------|----------------|--------------|
| Time | 2 min | 15+ min |
| Dependencies | Auto | Manual |
| TypeScript | One flag | Multiple steps |
| Tailwind | One flag | Multiple steps |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT - see [LICENSE](LICENSE)

---

[npm-badge]: https://img.shields.io/npm/v/create-next-app
[npm-url]: https://www.npmjs.com/package/create-next-app
[license-badge]: https://img.shields.io/npm/l/create-next-app
[build-badge]: https://github.com/vercel/next.js/actions/workflows/create-next-app.yml/badge.svg
```

## Key Takeaways

1. **One-liner** - Clear purpose
2. **Multiple install options** - npm, yarn, pnpm, npx
3. **Flags table** - Clear documentation
4. **Terminal output shown** - Interactive mode example
5. **Troubleshooting** - Common issues
6. **Comparison** - Why this vs manual
