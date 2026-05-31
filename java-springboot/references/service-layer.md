# Service Layer

## Business Logic

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

## Transaction Management

```java
@Service
public class OrderService {

    // Transaction on method — sufficient for most cases
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
