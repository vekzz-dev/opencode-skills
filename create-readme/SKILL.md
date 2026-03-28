---
name: create-readme
description: >
  Create comprehensive, well-structured README.md files adapted to the project type.
  Trigger: When user asks to create README, generate documentation, or initialize project docs.
license: Apache-2.0
metadata:
  author: vekzz-dev
  version: "1.0"
---

## When to Use

- Creating a new README.md file
- User asks to "create README" or "generate documentation"
- Initializing project documentation
- Need help structuring project docs

## Instructions

### Step 1: Analyze the Project

First, explore the project to understand:

1. **Technology Stack**: Detect package manager and language
   - `package.json` → Node.js/npm library or app
   - `pom.xml` / `build.gradle` → Java/Maven/Gradle
   - `requirements.txt` / `setup.py` / `pyproject.toml` → Python
   - `go.mod` → Go
   - `Cargo.toml` → Rust
   - `composer.json` → PHP

2. **Framework**: Detect specific frameworks
   - `spring-boot` in pom.xml → Spring Boot
   - `react`, `vue`, `angular` in package.json → Frontend framework
   - `fastapi`, `django`, `flask` in requirements.txt → Python web
   - `express` in package.json → Express.js
   - `next`, `nuxt`, `astro` in package.json → Meta-frameworks

3. **Project Purpose**: Quick Start, library, CLI tool, web app, etc.

### Step 2: Select Template

Use the appropriate reference based on project type:

| Project Type | Reference |
|-------------|-----------|
| npm/pip/maven library | [references/sections/library.md](references/sections/library.md) |
| CLI tool | [references/sections/cli-tool.md](references/sections/cli-tool.md) |
| Web application | [references/sections/webapp.md](references/sections/webapp.md) |
| Python project | [references/sections/python.md](references/sections/python.md) |
| Java project | [references/sections/java.md](references/sections/java.md) |
| Go project | [references/sections/go.md](references/sections/go.md) |
| Rust project | [references/sections/rust.md](references/sections/rust.md) |
| Ruby / Rails project | [references/sections/ruby.md](references/sections/ruby.md) |
| .NET / C# project | [references/sections/dotnet.md](references/sections/dotnet.md) |
| Docker project | [references/sections/docker.md](references/sections/docker.md) |

### Step 3: Generate README

Follow these rules:

1. **Be concise** - Don't write lengthy prose, use structure
2. **Working code** - All examples must work, no pseudocode
3. **GFM + Admonitions** - Use GitHub Flavored Markdown and `> [!NOTE]`, `> [!WARNING]`, etc.
4. **Minimal emojis** - Use sparingly for visual hierarchy
5. **Table of Contents** - For READMEs longer than ~50 lines
6. **No redundant sections** - Skip LICENSE, CONTRIBUTING, CHANGELOG (separate files)

### Step 4: Add Badges

Include relevant badges from [references/badges.md](references/badges.md):

- Package version
- Build/CI status
- License
- Language stats
- Downloads

### Step 5: Include Examples

Reference [references/examples/](references/examples/) for working examples:

- [library_example.md](references/examples/library_example.md) - npm/pip libraries
- [cli_example.md](references/examples/cli_example.md) - CLI tools
- [webapp_example.md](references/examples/webapp_example.md) - Web applications

## Commands

```bash
# Detect project type
ls -la | grep -E "package.json|pom.xml|build.gradle|requirements.txt|go.mod|Cargo.toml"

# Check package.json for Node.js project
cat package.json | grep -E '"name"|"version"|"description"|"main"|"bin"'

# Check pom.xml for Java project
cat pom.xml | grep -E '<artifactId>|<version>|<description>'

# Check go.mod for Go project
cat go.mod | head -5

# Check Cargo.toml for Rust project
cat Cargo.toml | grep -E '\[package\]|name|version|description'
```

## README Structure

### Required Sections

```
# <project-name>

One-line description of what the project does.

## Installation

Quick installation command

## Usage

Minimal working example

## License

One line
```

### Optional Sections (include as needed)

- **Badges** - At top, below title
- **Table of Contents** - If > 50 lines
- **Features** - Key capabilities
- **Requirements** - Prerequisites
- **Configuration** - Setup options
- **API Reference** - For libraries
- **Contributing** - Short reference (link to CONTRIBUTING.md)
- **Acknowledgments** - Third-party credits

## Examples Template

### Library

```markdown
# my-lib

A brief description of what this library does.

[![npm version][npm-badge]][npm-url]

## Installation

```bash
npm install my-lib
```

## Usage

```javascript
import { myFunction } from 'my-lib';

const result = myFunction('input');
console.log(result);
```

## API

| Function | Description | Returns |
|----------|-------------|---------|
| `myFunction(input)` | Does something | `string` |

## License

MIT - see [LICENSE](LICENSE)
```

### CLI Tool

```markdown
# my-cli

A brief description of what this CLI tool does.

[![npm version][npm-badge]][npm-url]

## Installation

```bash
npm install -g my-cli
```

## Usage

```bash
my-cli --help
my-cli command --option value
```

## Commands

| Command | Description |
|---------|-------------|
| `my-cli init` | Initialize project |
| `my-cli build` | Build the project |

## License

MIT - see [LICENSE](LICENSE)
```

### Web Application

```markdown
# my-app

A brief description of this web application.

[![Build status][build-badge]][build-url]

## Features

- Feature 1
- Feature 2
- Feature 3

## Quick Start

```bash
npm install
npm run dev
```

Open http://localhost:3000

## Deployment

Deployment instructions...

## License

MIT - see [LICENSE](LICENSE)
```

## Key Principles

1. **Answer the 4 questions in 60 seconds:**
   - What does this do?
   - How do I install it?
   - How do I use it?
   - Should I trust it?

2. **Show working code first** - Don't explain before showing

3. **Use tables** - For API, options, commands

4. **Keep it updated** - Outdated READMEs lose trust

5. **Be helpful** - Think like a new user, not the maintainer

## Resources

- [Project Types Detection](references/project-types.md)
- [Section Templates](references/sections/)
- [Examples](references/examples/)
- [Badges](references/badges.md)
