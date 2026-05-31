# Web Layer

## RESTful Controller

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

## DTOs — Don't Expose Entities

```java
// ❌ BAD — Exposing JPA entity directly
@RestController
public class UserController {
    @GetMapping
    public List<User> getUsers() {
        return userRepository.findAll(); // Returns JPA entity with lazy loads!
    }
}

// ✅ GOOD — Using DTO
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

## Validation

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

## Global Exception Handler

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
