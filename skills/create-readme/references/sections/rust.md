# Rust Project README Template

Use this template for Rust projects (libraries, CLI tools, applications).

## Structure

```markdown
# <project-name>

One-line description of what this Rust project does.

[![Crates.io][crates-badge]][crates-url]
[![Rust version][rust-badge]][rust-url]
[![License][license-badge]][license-url]

## Why?

Explain the problem this project solves.

## Installation

### Cargo Install

```bash
cargo install project-name
```

### From Source

```bash
git clone https://github.com/username/project-name.git
cd project-name
cargo install --path .
```

### As Dependency

```toml
[dependencies]
project-name = "1.0"
```

## Quick Start

```rust
use project_name::main_function;

fn main() {
    let result = main_function("input");
    println!("{}", result);
}
```

## Usage

### CLI

```bash
$ project-name --help
$ project-name command --option value
```

### As Library

```rust
use project_name::{Client, Config};

let config = Config::default();
let client = Client::new(config);
```

## Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `DEBUG` | Enable debug mode | `false` |
| `CONFIG_PATH` | Config file path | `~/.project-name.toml` |

### Config File

```toml
[project]
option = true
value = "test"
```

## API Reference

### Crate

```rust
use project_name::{Client, Config, Error};

pub fn new(config: Config) -> Result<Client, Error>
pub fn process(&self, input: &str) -> Result<String, Error>
```

### Structs

```rust
pub struct Config {
    pub option: bool,
    pub value: String,
}
```

## Examples

### Basic Example

```rust
use project_name::prelude::*;

fn main() -> Result<()> {
    let config = Config::builder()
        .option(true)
        .build();
    
    let client = Client::new(config)?;
    let result = client.process("input")?;
    
    println!("{}", result);
    Ok(())
}
```

## Requirements

- Rust 1.70+
- Cargo 1.70+

## Development

```bash
# Clone
git clone https://github.com/username/project-name.git
cd project-name

# Run tests
cargo test

# Build
cargo build --release

# Clippy
cargo clippy -- -D warnings
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT - see [LICENSE](LICENSE)
```

---

## Rust-Specific Elements

1. **Cargo.toml** - Dependencies and metadata
2. **cargo install** - Installation command
3. **Cargo detection** in project-types
4. **Cargo.toml** specific templates
5. **cargo test/clippy** - Testing commands
6. **Crates.io badges** - Package registry
7. **Struct/impl patterns** - Rust idioms
