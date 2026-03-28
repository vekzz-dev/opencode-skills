# Ruby Project README Template

Use this template for Ruby projects (gems, Rails apps, Ruby libraries).

## Structure

```markdown
# <project-name>

One-line description of what this Ruby project does.

[![RubyGems][gem-badge]][gem-url]
[![Ruby version][ruby-badge]][ruby-url]
[![License][license-badge]][license-url]

## Why?

Explain the problem this project solves.

## Installation

### As Gem

```bash
gem install project-name
```

### Bundler

```ruby
# Gemfile
gem 'project-name'
```

```bash
bundle install
```

### From Source

```bash
git clone https://github.com/username/project-name.git
cd project-name
gem build project-name.gemspec
gem install project-name.gem
```

## Quick Start

```ruby
require 'project_name'

result = ProjectName.do_something('input')
puts result
```

## Usage

### Rails

```ruby
# config/initializers/project_name.rb
ProjectName.configure do |config|
  config.option = true
end
```

### Standalone

```ruby
require 'project_name'

client = ProjectName::Client.new
result = client.process(input)
```

## Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `PROJECT_NAME_DEBUG` | Enable debug | `false` |
| `PROJECT_NAME_CONFIG` | Config path | `~/.project-name.rb` |

### Initializer (Rails)

```ruby
ProjectName.configure do |config|
  config.api_key = ENV['API_KEY']
  config.option = true
end
```

### Config File

```ruby
# ~/.project-name.rb
ProjectName.configure do |config|
  config.option = true
end
```

## API Reference

### Main Class

```ruby
ProjectName.configure { |config| ... }
ProjectName.do_something(input)
```

### Client

```ruby
client = ProjectName::Client.new(options)
client.process(input)
client.method_name(arg)
```

## Examples

### Basic Usage

```ruby
require 'project_name'

# Simple call
result = ProjectName.process('input')
puts result

# With configuration
ProjectName.configure do |config|
  config.debug = true
end
```

### Rails Integration

```ruby
class ApplicationController < ActionController::Base
  def authenticate
    ProjectName::Auth.verify token
  end
end
```

### Rake Tasks

```bash
rake project_name:task
```

## Testing

```bash
# Run tests
rake spec
# or
rspec

# With coverage
COVERAGE=1 rake spec
```

## Requirements

- Ruby 3.0+
- RubyGems

## Development

```bash
# Clone
git clone https://github.com/username/project-name.git
cd project-name

# Install dependencies
bundle install

# Run tests
rake spec

# Build gem
gem build project-name.gemspec
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT - see [LICENSE](LICENSE)
```

---

## Ruby-Specific Elements

1. **Gemfile** - Dependencies (Bundler)
2. **gemspec** - Gem metadata
3. **gem install** - Installation command
4. **Ruby/Rails detection** in project-types
5. **Bundler patterns** - Gemfile, bundle install
6. **Rails integration** - Initializers, generators
7. **Rake tasks** - Common in Ruby projects
8. **RSpec/Minitest** - Testing frameworks
