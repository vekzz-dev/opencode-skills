---
name: java-springboot
description: >
  Comprehensive best practices for developing high-quality Spring Boot applications with production-ready patterns.
  Trigger: When developing Spring Boot applications, need best practices, or working with Spring framework.
license: Apache-2.0
metadata:
  author: vekzz-dev
  version: "1.0"
---

## When to Use

- Developing Spring Boot applications
- Need best practices for Spring Boot
- Configuring Spring Boot projects
- Working with Spring framework components

## Instructions

### Project Setup & Structure

#### Build Tool

Use Maven or Gradle for dependency management.

```xml
<!-- Maven: pom.xml -->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.2.0</version>
</parent>
```

```groovy
// Gradle: build.gradle
plugins {
    id 'java'
    id 'org.springframework.boot' version '3.2.0'
}
```

#### Package Structure

**Recommended: Feature/Domain-based**

```
com.example.app/
├── order/
│   ├── OrderController.java
│   ├── OrderService.java
│   ├── OrderRepository.java
│   ├── Order.java
│   └── OrderDTO.java
├── user/
│   └── ...
└── shared/
    ├── config/
    └── exception/
```

**Avoid: Layer-based**

```
com.example.app/
├── controller/  ❌
├── service/    ❌
├── repository/ ❌
└── entity/     ❌
```

#### Starters

Use Spring Boot starters to simplify dependencies:

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-validation</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
</dependencies>
```

### Dependency Injection

#### Constructor Injection (Recommended)

```java
@Service
public class OrderService {

    private final OrderRepository orderRepository;
    private final PaymentService paymentService;
    private final NotificationService notificationService;

    // Constructor injection - dependencies explicit and testable
    public OrderService(OrderRepository orderRepository,
                        PaymentService paymentService,
                        NotificationService notificationService) {
        this.orderRepository = orderRepository;
        this.paymentService = paymentService;
        this.notificationService = notificationService;
    }
}
```

#### Using Lombok

```java
@Service
@RequiredArgsConstructor
public class OrderService {

    private final OrderRepository orderRepository;
    private final PaymentService paymentService;
}
```

#### Component Stereotypes

| Annotation | Use For |
|-----------|---------|
| `@Component` | Generic Spring-managed beans |
| `@Service` | Business logic layer |
| `@Repository` | Data access/DB operations |
| `@Controller` / `@RestController` | Web controllers |
| `@Configuration` | Configuration classes |

### Configuration

#### application.yml Structure

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

#### Type-Safe Configuration

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

#### Profile-Specific Configuration

```
src/main/resources/
├── application.yml          # Default
├── application-dev.yml     # Development
├── application-staging.yml # Staging
└── application-prod.yml    # Production
```

#### Secrets Management

**DO NOT hardcode secrets:**

```java
// ❌ BAD
private static final String API_KEY = "secret-key-123";

// ✅ GOOD - Environment variable
private String apiKey = System.getenv("API_KEY");

// ✅ GOOD - Configuration property
@Value("${api.key}")
private String apiKey;
```

### Web Layer (Controllers)

#### RESTful Controller

```java
@RestController
@RequestMapping("/api/orders")
@RequiredArgsConstructor
public class OrderController {

    private final OrderService orderService;
    private final OrderMapper mapper;

    @GetMapping
    public ResponseEntity<Page<OrderResponse>> getOrders(
            @RequestParam(defaultValue = "0") int page,
            @RequestParam(defaultValue = "20") int size,
            @RequestParam(required = false) String status) {
        
        Pageable pageable = PageRequest.of(page, size);
        Page<Order> orders = orderService.findOrders(status, pageable);
        
        return ResponseEntity.ok(orders.map(mapper::toResponse));
    }

    @GetMapping("/{id}")
    public ResponseEntity<OrderResponse> getOrder(@PathVariable Long id) {
        return orderService.findById(id)
            .map(order -> ResponseEntity.ok(mapper.toResponse(order)))
            .orElse(ResponseEntity.notFound().build());
    }

    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)
    public OrderResponse createOrder(@Valid @RequestBody CreateOrderRequest request) {
        Order order = mapper.toEntity(request);
        Order saved = orderService.create(order);
        return mapper.toResponse(saved);
    }
}
```

#### DTOs - Don't Expose Entities

```java
// ❌ BAD - Exposing JPA entity directly
@RestController
public class UserController {
    @GetMapping
    public List<User> getUsers() {
        return userRepository.findAll(); // Returns JPA entity with lazy loads!
    }
}

// ✅ GOOD - Using DTO
public record UserResponse(
    Long id,
    String name,
    String email,
    Instant createdAt
) {}

@RestController
public class UserController {
    @GetMapping
    public List<UserResponse> getUsers() {
        return userRepository.findAll().stream()
            .map(user -> new UserResponse(
                user.getId(),
                user.getName(),
                user.getEmail(),
                user.getCreatedAt()
            ))
            .toList();
    }
}
```

#### Validation

```java
public record CreateOrderRequest(
    @NotBlank(message = "Product name is required")
    @Size(max = 100)
    String productName,
    
    @NotNull(message = "Quantity is required")
    @Min(1)
    @Max(1000)
    Integer quantity,
    
    @NotNull
    @Valid
    AddressDTO address
) {}
```

#### Global Exception Handler

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ErrorResponse> handleValidationErrors(
            MethodArgumentNotValidException ex) {
        
        List<FieldError> errors = ex.getBindingResult().getFieldErrors();
        List<String> messages = errors.stream()
            .map(error -> error.getField() + ": " + error.getDefaultMessage())
            .toList();
        
        return ResponseEntity
            .badRequest()
            .body(new ErrorResponse("VALIDATION_ERROR", messages));
    }

    @ExceptionHandler(OrderNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleNotFound(OrderNotFoundException ex) {
        return ResponseEntity
            .status(HttpStatus.NOT_FOUND)
            .body(new ErrorResponse("NOT_FOUND", ex.getMessage()));
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGeneric(Exception ex) {
        return ResponseEntity
            .status(HttpStatus.INTERNAL_SERVER_ERROR)
            .body(new ErrorResponse("INTERNAL_ERROR", "An unexpected error occurred"));
    }
}

public record ErrorResponse(String code, String message) {}
public record ErrorResponse(String code, List<String> messages) {}
```

### Service Layer

#### Business Logic

```java
@Service
@RequiredArgsConstructor
@Transactional
public class OrderService {

    private final OrderRepository orderRepository;
    private final PaymentService paymentService;
    private final InventoryService inventoryService;

    public Order createOrder(Order order) {
        // Validate
        validateOrder(order);
        
        // Reserve inventory
        inventoryService.reserve(order.getItems());
        
        // Process payment
        PaymentResult payment = paymentService.process(order.getPayment());
        
        // Save order with payment status
        order.setStatus(OrderStatus.PENDING);
        Order saved = orderRepository.save(order);
        
        // Async notification (don't block)
        notificationService.sendOrderCreated(saved.getId());
        
        return saved;
    }

    private void validateOrder(Order order) {
        if (order.getItems().isEmpty()) {
            throw new IllegalArgumentException("Order must have items");
        }
    }
}
```

#### Transaction Management

```java
@Service
public class OrderService {

    // Transaction on method - sufficient for most cases
    @Transactional
    public void processOrder(Long orderId) { }

    // Specify propagation for nested transactions
    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void logOrderAction(Long orderId) { }

    // Rollback for specific exceptions
    @Transactional(rollbackFor = {BusinessException.class})
    public void doSomething() { }
    
    // No rollback for specific exceptions
    @Transactional(noRollbackFor = ValidationException.class)
    public void lenientOperation() { }
}
```

### Data Layer

#### Repository

```java
@Repository
public interface OrderRepository extends JpaRepository<Order, Long> {

    // Method query
    List<Order> findByStatus(OrderStatus status);
    
    // Custom query
    @Query("SELECT o FROM Order o WHERE o.status = :status AND o.createdAt > :since")
    List<Order> findRecentByStatus(@Param("status") OrderStatus status,
                                   @Param("since") Instant since);
    
    // Native query
    @Query(value = "SELECT * FROM orders WHERE status = :status", 
           nativeQuery = true)
    List<Order> findByStatusNative(@Param("status") String status);
    
    // Projection
    List<OrderSummary> findByCustomerId(Long customerId);
    
    // Optional
    Optional<Order> findByOrderNumber(String orderNumber);
}

public interface OrderSummary {
    String getStatus();
    Instant getCreatedAt();
    BigDecimal getTotal();
}
```

#### Use Projections for Performance

```java
// ❌ BAD - Fetches entire entity
@Entity
public class Order {
    @OneToMany(fetch = FetchType.LAZY)
    private List<OrderItem> items; // Unnecessary for summary!
}

// ✅ GOOD - Use projection
public interface OrderDTO {
    Long getId();
    String getStatus();
    BigDecimal getTotal();
}

@Repository
public interface OrderRepository {
    List<OrderDTO> findAllSummaries();
}
```

### Logging

#### SLF4J Best Practices

```java
@Service
@Slf4j
public class OrderService {

    // ✅ Parameterized logging - avoids string concatenation
    public void processOrder(Long orderId) {
        log.info("Processing order: {}", orderId);
        log.debug("Order details: id={}, status={}", orderId, status);
    }

    // ✅ Exception logging with context
    public void handleError(Long orderId, Exception ex) {
        log.error("Failed to process order: {}, error: {}", orderId, ex.getMessage(), ex);
    }

    // ❌ BAD - String concatenation
    public void badPractice() {
        log.info("Order status: " + status); // Always evaluates even if disabled
    }
}
```

### Security

#### Spring Security Configuration

```java
@Configuration
@EnableWebSecurity
@RequiredArgsConstructor
public class SecurityConfig {

    private final JwtAuthenticationFilter jwtFilter;
    private final CustomUserDetailsService userDetailsService;

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .csrf(AbstractHttpConfigurer::disable)
            .sessionManagement(session -> 
                session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/auth/**").permitAll()
                .requestMatchers("/api/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated())
            .addFilterBefore(jwtFilter, UsernamePasswordAuthenticationFilter.class);
        
        return http.build();
    }
}
```

#### Password Encoding

```java
@Configuration
public class SecurityConfig {

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder(12); // Work factor 12
    }
}
```

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

See [spring-boot-testing](../spring-boot-testing/SKILL.md) skill for comprehensive testing guide.

### Quick Reference

| Test Type | Annotation | Use For |
|-----------|------------|---------|
| Unit | `@ExtendWith(MockitoExtension.class)` | Service logic |
| Web | `@WebMvcTest` | Controllers |
| Data | `@DataJpaTest` | Repositories |
| Integration | `@SpringBootTest` | Full flow |
| Slice | `@RestClientTest` | REST clients |

## Advanced Topics

### Async Processing

```java
@Service
public class NotificationService {

    @Async
    @AsyncExecutor("taskExecutor")
    public CompletableFuture<Void> sendAsync(String message) {
        // Runs in background thread pool
        emailSender.send(message);
        return CompletableFuture.completedFuture(null);
    }
}

@Configuration
@EnableAsync
public class AsyncConfig {
    @Bean
    public Executor taskExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(5);
        executor.setMaxPoolSize(10);
        executor.setQueueCapacity(100);
        executor.setThreadNamePrefix("async-");
        executor.initialize();
        return executor;
    }
}
```

### Caching

```java
@Service
@Slf4j
public class ProductService {

    @Cacheable(value = "products", key = "#productId")
    public Product getProduct(Long productId) {
        log.info("Fetching product: {}", productId);
        return productRepository.findById(productId)
            .orElseThrow(() -> new ProductNotFoundException(productId));
    }

    @CacheEvict(value = "products", key = "#product.id")
    public Product updateProduct(Product product) {
        return productRepository.save(product);
    }
}

@Configuration
@EnableCaching
public class CacheConfig {
    @Bean
    public CacheManager cacheManager() {
        return new ConcurrentMapCacheManager("products");
    }
}
```

### API Versioning

```java
@RestController
@RequestMapping("/api/v1")
public class OrderControllerV1 {

    @GetMapping("/orders")
    public List<OrderResponse> getOrders() { /* ... */ }
}

@RestController
@RequestMapping("/api/v2")
public class OrderControllerV2 {

    @GetMapping("/orders")
    public List<OrderResponseV2> getOrders(
            @RequestParam(defaultValue = "0") int page,
            @RequestParam(defaultValue = "20") int size) { /* ... */ }
}
```

### Pagination

```java
@GetMapping("/orders")
public Page<OrderResponse> getOrders(
        @PageableDefault(size = 20, sort = "createdAt") Pageable pageable) {
    return orderService.findAll(pageable).map(mapper::toResponse);
}
```

## Monitoring & Observability

### Actuator Endpoints

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

### Health Indicators

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

## Resources

- [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- [Spring Framework Documentation](https://spring.io/projects/spring-framework)
- [Spring Security Documentation](https://spring.io/projects/spring-security)

## Context7 Integration

When you need up-to-date information on Spring Boot:

1. **Resolve the libraryId** - Use `context7_resolve-library-id` with:
   - `libraryName`: "spring boot" or "spring framework"
   - `query`: what you're going to do (e.g., "configuration properties", "custom health check")

2. **Query the docs** - Use `context7_query-docs` with:
   - `libraryId`: the ID from the previous step (e.g., "/spring/spring-boot")
   - `query`: your specific question

**Before answering** about Spring Boot APIs, configurations, or best practices, consult Context7 to get up-to-date information.

## Key Principles

1. **Constructor Injection** - Always use it for testability
2. **DTOs** - Never expose JPA entities to API
3. **Validation** - Use Bean Validation everywhere
4. **Transactions** - Keep them short and at service layer
5. **Logging** - Use parameterized logging
6. **Security** - Never hardcode secrets
7. **Testing** - Use appropriate test slice
8. **Configuration** - Externalize everything
9. **Monitoring** - Add Actuator and health checks
