# Logging

## SLF4J Best Practices

```java
@Service
@Slf4j
public class OrderService {

    // ✅ Parameterized logging — avoids string concatenation
    public void processOrder(Long orderId) {
        log.info("Processing order: {}", orderId);
        log.debug("Order details: id={}, status={}", orderId, status);
    }

    // ✅ Exception logging with context
    public void handleError(Long orderId, Exception ex) {
        log.error("Failed to process order: {}, error: {}", orderId, ex.getMessage(), ex);
    }

    // ❌ BAD — String concatenation
    public void badPractice() {
        log.info("Order status: " + status); // Always evaluates even if disabled
    }
}
```
