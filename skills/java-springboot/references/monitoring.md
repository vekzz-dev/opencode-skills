# Monitoring & Observability

## Actuator Endpoints

```yaml
management:
  endpoints:
    web:
      exposure:
        include: health, info, metrics, prometheus
  endpoint:
    health:
      show-details: always
  health:
    livenessState:
      enabled: true
    readinessState:
      enabled: true
```

## Health Indicators

```java
@Component
public class DatabaseHealthIndicator implements ReactiveHealthIndicator {

    @Override
    public Mono<Health> health() {
        return Mono.just(Health.up()
            .withDetail("database", "UP")
            .build());
    }
}
```
