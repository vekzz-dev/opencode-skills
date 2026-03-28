# Docker Project README Template

Use this template for Docker projects (Dockerfiles, docker-compose, containerized apps).

## Structure

```markdown
# <project-name>

One-line description of what this Docker project provides.

[![Docker Image Size][docker-size]][docker-url]
[![Docker Pulls][docker-pulls]][docker-url]
[![License][license-badge]][license-url]

## Why?

Explain what this container solves.

## Quick Start

### Pull Image

```bash
docker pull username/project-name:latest
```

### Run Container

```bash
docker run -d \
  --name project-name \
  -p 8080:8080 \
  username/project-name:latest
```

Open http://localhost:8080

## Installation

### From Docker Hub

```bash
docker pull username/project-name
```

### Build from Source

```bash
git clone https://github.com/username/project-name.git
cd project-name
docker build -t project-name .
```

## Usage

### Basic Run

```bash
docker run project-name
```

### With Environment Variables

```bash
docker run -d \
  -e ENV_VAR=value \
  -e API_KEY=secret \
  project-name
```

### With Volumes

```bash
docker run -d \
  -v /host/path:/container/path \
  -v data:/data \
  project-name
```

### Using docker-compose

```bash
docker-compose up -d
```

## Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `PORT` | Server port | `8080` |
| `DEBUG` | Enable debug | `false` |
| `CONFIG_PATH` | Config file path | `/app/config` |

### Docker Compose

```yaml
version: '3.8'

services:
  app:
    image: username/project-name
    ports:
      - "8080:8080"
    environment:
      - DEBUG=false
      - DATABASE_URL=postgres://user:pass@db:5432/app
    volumes:
      - data:/data

  db:
    image: postgres:16
    environment:
      - POSTGRES_DB=app
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=pass
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  data:
  postgres_data:
```

## Build Arguments

| Arg | Description | Default |
|-----|-------------|---------|
| `BASE_IMAGE` | Parent image | python:3.11-slim |
| `USER` | Run as non-root | appuser |

## Volumes

| Volume | Description |
|--------|-------------|
| `/data` | Application data |
| `/config` | Configuration files |
| `/logs` | Log files |

## Ports

| Port | Description |
|------|-------------|
| `8080` | HTTP server |
| `8443` | HTTPS server |

## Examples

### Development Mode

```bash
docker run -d \
  --name project-dev \
  -p 3000:3000 \
  -e NODE_ENV=development \
  -v $(pwd):/app \
  project-name:dev
```

### Production Mode

```bash
docker run -d \
  --name project-prod \
  -p 80:8080 \
  -e NODE_ENV=production \
  --restart unless-stopped \
  project-name:latest
```

### With Custom Config

```bash
docker run -d \
  -v $(pwd)/config.yaml:/app/config/custom.yaml:ro \
  project-name
```

## Security

### Best Practices

- Run as non-root user
- Use official base images
- Scan for vulnerabilities regularly
- Don't include secrets in image

### Scan for Vulnerabilities

```bash
# Trivy
trivy image username/project-name

# Docker Scout
docker scout cves username/project-name
```

## Docker Compose Variants

### Development

```yaml
version: '3.8'

services:
  app:
    build:
      context: .
      target: development
    volumes:
      - .:/app
    ports:
      - "3000:3000"
    environment:
      - DEBUG=true
```

### Production

```yaml
version: '3.8'

services:
  app:
    build:
      context: .
      target: production
    ports:
      - "80:8080"
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8080/health"]
```

## Requirements

- Docker 24.0+
- Docker Compose 2.20+ (optional)

## Building

```bash
# Build image
docker build -t project-name .

# Build multi-platform
docker buildx build --platform linux/amd64,linux/arm64 -t project-name .

# Build with args
docker build --build-arg VERSION=1.0.0 -t project-name .
```

## Development

```bash
# Rebuild on changes
docker compose watch

# View logs
docker compose logs -f

# Shell into container
docker compose exec app sh
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT - see [LICENSE](LICENSE)
```

---

## Docker-Specific Elements

1. **Dockerfile** - Container definition
2. **docker-compose.yml** - Multi-container setup
3. **docker build** - Build commands
4. **Docker detection** in project-types
5. **Environment variables** - Container configuration
6. **Volumes** - Data persistence
7. **Ports** - Network exposure
8. **Multi-stage builds** - Optimization
9. **Security scanning** - Best practices
10. **docker-compose variants** - Dev/prod modes
