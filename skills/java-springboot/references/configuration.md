# Configuration

## application.yml Structure

```yaml
server:
  port: 8080
  servlet:
    context-path: /api

spring:
  application:
    name: my-app
  datasource:
    url: jdbc:postgresql://localhost:5432/mydb
    username: ${DB_USERNAME}
    password: ${DB_PASSWORD}
  jpa:
    hibernate:
      ddl-auto: validate
    show-sql: false
    properties:
      hibernate:
        format_sql: true

logging:
  level:
    com.example: DEBUG
    org.springframework.web: INFO
```

## Type-Safe Configuration

```java
@Configuration
@ConfigurationProperties(prefix = "app")
@Data
public class AppProperties {

    private String name;
    private int maxRetries;
    private String[] allowedOrigins;

    @NestedConfigurationProperty
    private Retry retry = new Retry();

    @Data
    public static class Retry {
        private int maxAttempts = 3;
        private long delay = 1000;
    }
}
```

```java
@EnableConfigurationProperties(AppProperties.class)
@SpringBootApplication
public class Application { }
```

## Profile-Specific Configuration

```
src/main/resources/
├── application.yml          # Default
├── application-dev.yml     # Development
├── application-staging.yml # Staging
└── application-prod.yml    # Production
```

## Secrets Management

**DO NOT hardcode secrets:**

```java
// ❌ BAD
private static final String API_KEY = "secret-key-123";

// ✅ GOOD — Environment variable
private String apiKey = System.getenv("API_KEY");

// ✅ GOOD — Configuration property
@Value("${api.key}")
private String apiKey;
```
