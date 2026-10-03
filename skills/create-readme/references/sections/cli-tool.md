# CLI Tool README Template

Use this template for command-line tools.

## Structure

```markdown
# <cli-name>

Command-line tool for <what it does>.

[![npm version][npm-badge]][npm-url]
[![Build Status][build-badge]][build-url]
[![License][license-badge]][license-url]

## Why?

Brief explanation of what problem this CLI solves.

## Installation

```bash
# npm global
npm install -g <cli-name>

# Homebrew
brew install <cli-name>

# Cargo (Rust)
cargo install <cli-name>

# Go
go install github.com/username/cli-name@latest
```

## Quick Start

```bash
<cli-name> --help
<cli-name> init
<cli-name> build --production
```

## Commands

### init

Initialize a new project.

```bash
<cli-name> init [project-name]
```

**Options:**

| Flag | Alias | Default | Description |
|------|-------|---------|-------------|
| `--template` | `-t` | `default` | Template to use |
| `--force` | `-f` | `false` | Overwrite existing |

**Example:**

```bash
<cli-name> init my-project -t react
```

### build

Build the project.

```bash
<cli-name> build [options]
```

**Options:**

| Flag | Alias | Default | Description |
|------|-------|---------|-------------|
| `--production` | `-p` | `false` | Production build |
| `--watch` | `-w` | `false` | Watch mode |
| `--out-dir` | `-o` | `dist` | Output directory |

**Example:**

```bash
<cli-name> build --production -o ./build
```

### deploy

Deploy the project.

```bash
<cli-name> deploy [environment]
```

**Environments:** `staging`, `production`

**Example:**

```bash
<cli-name> deploy production
```

## Configuration

### Config File

Create `.<cli-name>rc` or `.<cli-name>.json`:

```json
{
  "apiKey": "your-api-key",
  "region": "us-east-1",
  "output": "table"
}
```

### Environment Variables

| Variable | Description |
|----------|-------------|
| `<CLI_NAME>_API_KEY` | API authentication |
| `<CLI_NAME>_DEBUG` | Enable debug output |
| `<CLI_NAME>_CONFIG` | Custom config path |

## Examples

### Basic Usage

```bash
# Initialize project
$ my-cli init my-app

✓ Created project my-app
✓ Installed dependencies

# Build project  
$ my-cli build

✓ Compiled 42 files in 2.3s

# Deploy
$ my-cli deploy production

✓ Deployed to https://my-app.example.com
```

### With Options

```bash
my-cli build --production --out-dir ./dist --no-cache
```

## Requirements

- Node.js 18+
- npm 9+ (for npm installation)

## Autocompletion

### Bash

```bash
# Add to ~/.bashrc
source <(my-cli completion bash)
```

### Zsh

```bash
# Add to ~/.zshrc
source <(my-cli completion zsh)
```

### Fish

```bash
my-cli completion fish > ~/.config/fish/completions/my-cli.fish
```

## Programmatic Usage

Some CLI tools can be used as modules:

```javascript
import { build, deploy } from 'my-cli';

await build({ production: true });
await deploy('production');
```

## Comparison

| Feature | This CLI | Alternative |
|---------|----------|-------------|
| Speed | Fast | Faster |
| Config | JSON | YAML |
| Plugins | ✅ | ❌ |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT - see [LICENSE](LICENSE)
```

## Key Elements

1. **Installation** - Multiple package managers
2. **Quick Start** - One-liner that works
3. **Commands** - Table format with examples
4. **Configuration** - Config file + ENV vars
5. **Examples** - Terminal output shown
6. **Autocompletion** - Shell-specific
7. **Programmatic** - If applicable

## Common CLI Frameworks

| Framework | Badge |
|-----------|-------|
| Commander | Popular Node CLI |
| oclif | Node (Salesforce) |
| Click | Python |
| Cobra | Go |
| Clap | Rust |
