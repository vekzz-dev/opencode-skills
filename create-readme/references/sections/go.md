# Go Project README Template

Use this template for Go projects (libraries, CLI tools, applications).

## Structure

```markdown
# <project-name>

One-line description of what this Go project does.

[![Go version][go-badge]][go-url]
[![Module][module-badge]][module-url]
[![License][license-badge]][license-url]

## Why?

Explain the problem this project solves.

## Installation

### Go Install

```bash
go install github.com/username/project-name@latest
```

### From Source

```bash
git clone https://github.com/username/project-name.git
cd project-name
go install
```

## Quick Start

```go
package main

import (
    "fmt"
    "github.com/username/project-name"
)

func main() {
    result := projectName.DoSomething("input")
    fmt.Println(result)
}
```

## Usage

### CLI

```bash
$ project-name --help
$ project-name command --flag value
```

### As Library

```go
import "github.com/username/project-name"

func main() {
    // Use the package
}
```

## Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `DEBUG` | Enable debug mode | `false` |
| `CONFIG_PATH` | Config file path | `~/.project-name.yaml` |

### Config File

```yaml
project:
  name: value
  option: true
```

## API Reference

### Functions

| Function | Description | Returns |
|----------|-------------|---------|
| `New()` | Create new instance | `*Client` |
| `DoSomething(input)()` | Does something | `error` |

### Types

```go
type Config struct {
    Option bool
}
```

## Examples

### Basic Example

```go
package main

import "github.com/username/project-name"

func main() {
    client := projectName.New()
    err := client.Process()
    if err != nil {
        log.Fatal(err)
    }
}
```

## Requirements

- Go 1.21+
- Git

## Development

```bash
# Clone
git clone https://github.com/username/project-name.git
cd project-name

# Run tests
go test ./...

# Build
go build -o bin/ project-name
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT - see [LICENSE](LICENSE)
```

---

## Go-Specific Elements

1. **go.mod** - Module path and version
2. **go install** - Installation command
3. **go.mod** detection in project-types
4. **Environment variables** - Standard Go patterns
5. **go test** - Testing commands
6. **Module badges** - go.mod reference
