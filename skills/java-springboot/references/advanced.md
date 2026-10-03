# Advanced Topics

## Async Processing

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

## Caching

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

## API Versioning

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

## Pagination

```java
@GetMapping("/orders")
public Page<OrderResponse> getOrders(
        @PageableDefault(size = 20, sort = "createdAt") Pageable pageable) {
    return orderService.findAll(pageable).map(mapper::toResponse);
}
```
