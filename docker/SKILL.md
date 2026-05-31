---
name: docker
description: >
  Docker and containerization best practices: multi-stage builds, docker-compose, networking, volumes, security, and image optimization.
  Trigger: Docker, Dockerfile, docker-compose, container, image build, or containerization.
license: Apache-2.0
metadata:
  author: vekzz-dev
  version: "1.0"
---

## When to Use

- Writing or reviewing a Dockerfile
- Setting up docker-compose for dev/test/prod
- Optimizing image size or build time
- Container security, networking, or volume management
- Dockerizing a Spring Boot application

## Instructions

### 1. Multi-stage Builds for Spring Boot

```dockerfile
# Stage 1: Build with Maven
FROM eclipse-temurin:21-jdk-alpine AS builder
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline -B
COPY src src
RUN mvn package -DskipTests -B

# Stage 2: Extract layered JAR for cache efficiency
RUN java -Djarmode=layertools -jar target/app.jar extract --destination extracted

# Stage 3: Runtime
FROM eclipse-temurin:21-jre-alpine
RUN addgroup -S app && adduser -S app -G app
USER app
WORKDIR /app
COPY --from=builder /app/extracted/dependencies/ ./
COPY --from=builder /app/extracted/spring-boot-loader/ ./
COPY --from=builder /app/extracted/snapshot-dependencies/ ./
COPY --from=builder /app/extracted/application/ ./
EXPOSE 8080
ENTRYPOINT ["java", "org.springframework.boot.loader.launch.JarLauncher"]
```

Use **layered JARs** so dependency layers cache independently from application code.

### 2. Dockerfile Best Practices

| Practice | Why |
|----------|-----|
| Use specific tags (`21-jre-alpine`, not `latest`) | Reproducible builds |
| Prefer `eclipse-temurin` over `openjdk` | Actively maintained, security patched |
| Run as non-root (`USER app`) | Security — container breakout mitigation |
| Combine `RUN` commands | Reduce layers; `apk add --no-cache && rm -rf /var/cache/apk/*` |
| Use `COPY --chown=app:app` | Avoid permission issues at runtime |
| Set `EXPOSE` as documentation | Does not publish the port, just documents intent |
| Use `HEALTHCHECK` | Let orchestrators know container state |
| Prefer `exec` form (`["java", "-jar"]`) | Handles signals correctly (SIGTERM → graceful shutdown) |

### 3. docker-compose Patterns

**Dev environment with services:**

```yaml
services:
  app:
    build: .
    ports:
      - "8080:8080"
    environment:
      SPRING_PROFILES_ACTIVE: dev
      SPRING_DATASOURCE_URL: jdbc:postgresql://db:5432/mydb
      SPRING_DATASOURCE_USERNAME: app
      SPRING_DATASOURCE_PASSWORD: ${DB_PASSWORD}
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_started
    volumes:
      - ./target:/app/target  # hot reload in dev

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB: mydb
      POSTGRES_USER: app
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U app -d mydb"]
      interval: 5s
      timeout: 3s
      retries: 5

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"

volumes:
  pgdata:
```

| Pattern | Usage |
|---------|-------|
| `depends_on` + `condition: service_healthy` | Wait for DB to be ready before starting app |
| `.env` file | Keep secrets out of compose files |
| Named volumes | Persist data across restarts (db, cache) |
| Multi-stage `Dockerfile` | Dev builds fast, prod is slim |
| Profile-specific overrides | `docker-compose -f compose.yml -f compose.dev.yml` |

### 4. Decision Table

| Need | Approach |
|------|----------|
| Local dev with hot reload | `docker compose watch` or `spring-boot-devtools` with volume mount |
| CI build | Multi-stage with `--cache-from` |
| Production deploy | Slim runtime image, non-root, read-only root fs |
| Database in tests | `Testcontainers` (not docker-compose in tests) |
| Multiple microservices | `docker compose` with shared network |
| Kubernetes | Use the same image, add liveness/readiness probes |

### 5. Security

- **Never** hardcode secrets in Dockerfile or compose — use secrets mounts or env files
- `USER nonroot` — always
- `COPY --chown=nonroot:nonroot` — match the runtime user
- Read-only root filesystem: `--read-only --tmpfs /tmp`
- Use `docker scout` or `trivy` for vulnerability scanning
- No `curl`/`wget` in runtime image — reduces attack surface

## Commands

```bash
# Build with cache from registry
docker build --cache-from myapp:latest -t myapp:latest .

# Run with compose for dev
docker compose up -d

# Watch for hot reload (Docker Compose v2.22+)
docker compose watch

# Scan image for vulnerabilities
docker scout quick myapp:latest

# Run with read-only root fs
docker run --read-only --tmpfs /tmp myapp:latest
```

## Resources

- [Dockerfile reference](https://docs.docker.com/reference/dockerfile/)
- [Spring Boot layered JARs](https://docs.spring.io/spring-boot/reference/packaging/container-images/layered.html)
