# Project Types Detection

This guide helps identify the project type to generate the appropriate README structure.

## Detection by Package Manager

### Node.js / npm

**Files to check:**
- `package.json`
- `package-lock.json`

**Indicates:**
- npm library/package
- Node.js application
- CLI tool

**Keywords in package.json:**
- `"bin"` → CLI tool
- `"main"` → Library
- `"dependencies"` + `"scripts"` → Application

### Python

**Files to check:**
- `requirements.txt`
- `setup.py`
- `pyproject.toml`
- `setup.cfg`

**Indicates:**
- Python library
- Python application
- Django/Flask/FastAPI project

**Keywords:**
- `fastapi`, `flask`, `django` → Web framework
- `pytest` → Testing library

### Java / Maven / Gradle

**Files to check:**
- `pom.xml` (Maven)
- `build.gradle` / `build.gradle.kts` (Gradle)

**Indicates:**
- Java library
- Spring Boot application
- Java application

**Keywords in pom.xml:**
- `spring-boot-starter` → Spring Boot
- `junit` → Testing

### Go

**Files to check:**
- `go.mod`

**Indicates:**
- Go library
- Go application
- CLI tool

### Rust

**Files to check:**
- `Cargo.toml`

**Indicates:**
- Rust library
- Rust binary/application

### PHP

**Files to check:**
- `composer.json`

**Indicates:**
- PHP library
- Laravel/Symfony project

### Ruby

**Files to check:**
- `Gemfile`
- `*.gemspec`
- `Rakefile`

**Indicates:**
- Ruby gem
- Rails application
- Ruby library

**Keywords:**
- `rails` in Gemfile → Rails app
- `rspec` → Testing
- `rake` → Rake tasks

### .NET / C#

**Files to check:**
- `*.csproj`
- `*.sln`
- `*.fsproj`

**Indicates:**
- .NET library
- .NET application
- ASP.NET Core web app

**Keywords:**
- `<OutputType>Exe</OutputType>` → Console app
- `<PackageReference` → NuGet package
- `Microsoft.AspNetCore` → Web app

### Docker

**Files to check:**
- `Dockerfile`
- `docker-compose.yml`
- `docker-compose.yaml`

**Indicates:**
- Container image
- Containerized application
- Multi-container setup

**Keywords:**
- Multi-stage builds → Optimized image
- `docker-compose.override.yml` → Development config

## Detection by Framework

### Frontend Frameworks

**React:**
```json
"dependencies": {
  "react": "^18.0.0"
}
```

**Vue:**
```json
"dependencies": {
  "vue": "^3.0.0"
}
```

**Angular:**
```json
"dependencies": {
  "@angular/core": "^17.0.0"
}
```

**Svelte:**
```json
"dependencies": {
  "svelte": "^4.0.0"
}
```

### Meta-Frameworks

**Next.js:**
```json
"dependencies": {
  "next": "^14.0.0"
}
```

**Nuxt:**
```json
"dependencies": {
  "nuxt": "^3.0.0"
}
```

**Astro:**
```json
"dependencies": {
  "astro": "^4.0.0"
}
```

### Backend Frameworks

**Spring Boot (Java):**
```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

**Express.js (Node):**
```json
"dependencies": {
  "express": "^4.18.0"
}
```

**FastAPI (Python):**
```
fastapi>=0.100.0
```

**Django (Python):**
```
django>=4.0.0
```

**Flask (Python):**
```
flask>=2.0.0
```

## Project Purpose Detection

### Library/Package

- Has public API (functions, classes to import)
- Published to package manager (npm, pip, Maven Central)
- No run command, just importable

### CLI Tool

- Binary or executable
- Executable from terminal
- Has commands and options
- `bin` field in package.json

### Web Application

- Has frontend (HTML/CSS/JS)
- Runs on server
- Has routes/endpoints
- Often connects to database

### API/Backend

- Provides REST/GraphQL endpoints
- No frontend
- Often JSON responses

### Documentation Site

- Static site generator
- Content-focused
- Markdown files as source

## Quick Reference

| Type | Package Manager | Framework Examples | README Template |
|------|----------------|-------------------|----------------|
| Library (JS) | npm/pnpm | - | library.md |
| Library (Python) | pip | - | library.md |
| Library (Java) | Maven/Gradle | - | library.md |
| CLI Tool | npm global | commander, oclif | cli-tool.md |
| Web App | npm | React, Vue, Next.js | webapp.md |
| API | npm/pip | Express, FastAPI, Spring | webapp.md |
| Full Stack | npm | Next.js, Nuxt | webapp.md |

## Examples

### Example 1: npm Library

```json
{
  "name": "my-utils",
  "version": "1.0.0",
  "main": "dist/index.js",
  "types": "dist/index.d.ts"
}
```

**Template:** [library.md](sections/library.md)

### Example 2: CLI Tool

```json
{
  "name": "my-cli",
  "version": "1.0.0",
  "bin": {
    "my-cli": "./bin/cli.js"
  }
}
```

**Template:** [cli-tool.md](sections/cli-tool.md)

### Example 3: Next.js App

```json
{
  "name": "my-next-app",
  "scripts": {
    "dev": "next dev",
    "build": "next build"
  },
  "dependencies": {
    "next": "^14.0.0",
    "react": "^18.0.0"
  }
}
```

**Template:** [webapp.md](sections/webapp.md)

### Example 4: Python Library

```toml
[project]
name = "my-utils"
version = "1.0.0"

[project.optional-dependencies]
dev = ["pytest"]
```

**Template:** [library.md](sections/library.md) + [python.md](sections/python.md)

### Example 5: Spring Boot

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

**Template:** [java.md](sections/java.md)
