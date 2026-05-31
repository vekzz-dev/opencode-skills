# Data Layer

## Repository

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

## Use Projections for Performance

```java
// ❌ BAD — Fetches entire entity
@Entity
public class Order {
    @OneToMany(fetch = FetchType.LAZY)
    private List<OrderItem> items; // Unnecessary for summary!
}

// ✅ GOOD — Use projection
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
