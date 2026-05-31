# Dependency Injection

## Constructor Injection (Recommended)

```java
@Service
public class OrderService {

    private final OrderRepository orderRepository;
    private final PaymentService paymentService;
    private final NotificationService notificationService;

    // Constructor injection — dependencies explicit and testable
    public OrderService(OrderRepository orderRepository,
                        PaymentService paymentService,
                        NotificationService notificationService) {
        this.orderRepository = orderRepository;
        this.paymentService = paymentService;
        this.notificationService = notificationService;
    }
}
```

## Using Lombok

```java
@Service
@RequiredArgsConstructor
public class OrderService {

    private final OrderRepository orderRepository;
    private final PaymentService paymentService;
}
```

## Component Stereotypes

| Annotation | Use For |
|-----------|---------|
| `@Component` | Generic Spring-managed beans |
| `@Service` | Business logic layer |
| `@Repository` | Data access / DB operations |
| `@Controller` / `@RestController` | Web controllers |
| `@Configuration` | Configuration classes |
