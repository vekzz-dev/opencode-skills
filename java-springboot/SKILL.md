---
name: java-springboot
description: >
  Comprehensive best practices for developing high-quality Spring Boot applications with production-ready patterns.
  Trigger: When developing Spring Boot applications, need best practices, or working with Spring framework.
license: Apache-2.0
metadata:
  author: vekzz-dev
  version: "2.0"
---

## When to Use

- Developing Spring Boot applications
- Need best practices for Spring Boot
- Configuring Spring Boot projects
- Working with Spring framework components

## Core Principles

1. **Constructor Injection** — Always use it for testability. Never `@Autowired` on fields.
2. **DTOs** — Never expose JPA entities to the API layer. Use records or projection interfaces.
3. **Package by Feature** — Group by domain (order/, user/), not by layer (controller/, service/).
4. **Validation** — Use Bean Validation (`@Valid`, `@NotBlank`) on request DTOs.
5. **Transactions** — Keep them short and at the service layer with `@Transactional`.
6. **Logging** — Use parameterized logging (`{}`), never string concatenation.
7. **Security** — Never hardcode secrets. Use env vars and `@ConfigurationProperties`.
8. **Testing** — Use the narrowest test slice that gives you confidence.
9. **Configuration** — Externalize everything via `application.yml` and `@ConfigurationProperties`.
10. **Monitoring** — Add Actuator and health checks to every service.

## Reference Index

| Topic | Reference |
|---|---|
| Build tool, starters, package structure | [references/project-setup.md](references/project-setup.md) |
| Constructor injection, Lombok, stereotypes | [references/dependency-injection.md](references/dependency-injection.md) |
| application.yml, @ConfigurationProperties, profiles, secrets | [references/configuration.md](references/configuration.md) |
| REST controllers, DTOs, validation, exception handler | [references/web-layer.md](references/web-layer.md) |
| Business logic, @Transactional | [references/service-layer.md](references/service-layer.md) |
| Repositories, queries, projections | [references/data-layer.md](references/data-layer.md) |
| SLF4J logging patterns | [references/logging.md](references/logging.md) |
| Spring Security, JWT, password encoding | [references/security.md](references/security.md) |
| Async, caching, API versioning, pagination | [references/advanced.md](references/advanced.md) |
| Actuator, health indicators | [references/monitoring.md](references/monitoring.md) |
| Test slice quick reference | [references/testing.md](references/testing.md) |

## Commands

```bash
# Run Spring Boot application
mvn spring-boot:run

# Run with specific profile
mvn spring-boot:run -Dspring-boot.run.profiles=dev

# Build project
mvn clean package

# Run tests
mvn test

# Run specific test
mvn test -Dtest=OrderServiceTest
```

## Testing

For comprehensive testing guidance (test slices, MockMvcTester, Testcontainers, AssertJ), see the [spring-boot-testing](../spring-boot-testing/SKILL.md) skill.

### Quick Reference

| Test Type | Annotation | Use For |
|-----------|------------|---------|
| Unit | `@ExtendWith(MockitoExtension.class)` | Service logic |
| Web | `@WebMvcTest` | Controllers |
| Data | `@DataJpaTest` | Repositories |
| Integration | `@SpringBootTest` | Full flow |
| Slice | `@RestClientTest` | REST clients |

## Context7 Integration

When you need up-to-date information on Spring Boot:

1. **Resolve the libraryId** — Use `context7_resolve-library-id` with:
   - `libraryName`: "spring boot" or "spring framework"
   - `query`: what you're going to do (e.g., "configuration properties", "custom health check")

2. **Query the docs** — Use `context7_query-docs` with:
   - `libraryId`: the ID from the previous step (e.g., "/spring/spring-boot")
   - `query`: your specific question

**Before answering** about Spring Boot APIs, configurations, or best practices, consult Context7 to get up-to-date information.

## Resources

- [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- [Spring Framework Documentation](https://spring.io/projects/spring-framework)
- [Spring Security Documentation](https://spring.io/projects/spring-security)
