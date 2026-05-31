---
name: java-junit
description: >
  Best practices for JUnit 5 unit testing, including data-driven tests and modern patterns.
  Trigger: When writing JUnit tests, creating unit tests, or need testing best practices.
license: Apache-2.0
metadata:
  author: vekzz-dev
  version: "1.1"
---

## When to Use

- Writing JUnit 5 unit tests
- Creating parameterized tests
- Setting up test infrastructure
- Need testing best practices for Java

## Instructions

### Project Setup

- Use a standard Maven or Gradle project structure
- Place test source code in `src/test/java`
- Include dependencies for `junit-jupiter-api`, `junit-jupiter-engine`, and `junit-jupiter-params` for parameterized tests
- Use build tool commands to run tests: `mvn test` or `gradle test`

### Test Structure

- Test classes should have a `Test` suffix, e.g., `CalculatorTest` for a `Calculator` class
- Use `@Test` for test methods
- Follow the Arrange-Act-Assert (AAA) pattern
- Name tests using a descriptive convention, like `methodName_should_expectedBehavior_when_scenario`
- Use `@BeforeEach` and `@AfterEach` for per-test setup and teardown
- Use `@BeforeAll` and `@AfterAll` for per-class setup and teardown (must be static methods)
- Use `@DisplayName` to provide a human-readable name for test classes and methods

```java
class CalculatorTest {

    private Calculator calculator;

    @BeforeEach
    void setUp() {
        calculator = new Calculator();
    }

    @Test
    @DisplayName("Should add two positive numbers")
    void add_shouldReturnSum_whenBothPositive() {
        // Arrange
        int a = 3, b = 5;

        // Act
        int result = calculator.add(a, b);

        // Assert
        assertEquals(8, result);
    }

    @Test
    @DisplayName("Should throw exception when dividing by zero")
    void divide_shouldThrowException_whenDenominatorIsZero() {
        assertThrows(ArithmeticException.class,
            () -> calculator.divide(10, 0),
            "Division by zero should throw");
    }
}
```

### Standard Tests

- Keep tests focused on a single behavior
- Avoid testing multiple conditions in one test method
- Make tests independent and idempotent (can run in any order)
- Avoid test interdependencies

### Data-Driven (Parameterized) Tests

- Use `@ParameterizedTest` to mark a method as a parameterized test
- Use `@ValueSource` for simple literal values (strings, ints, etc.)
- Use `@MethodSource` to refer to a factory method that provides test arguments as a `Stream`, `Collection`, etc.
- Use `@CsvSource` for inline comma-separated values
- Use `@CsvFileSource` to use a CSV file from the classpath
- Use `@EnumSource` to use enum constants

```java
@ParameterizedTest
@ValueSource(ints = {1, 2, 3, 5, 8, 13})
@DisplayName("Should identify Fibonacci numbers")
void isFibonacci_shouldReturnTrue_forFibonacciNumbers(int number) {
    assertTrue(calculator.isFibonacci(number));
}

@ParameterizedTest
@CsvSource({
    "4, 2, 2",
    "10, 3, 1",
    "7, 5, 2"
})
@DisplayName("Should compute modulo for various inputs")
void modulo_shouldReturnRemainder(int dividend, int divisor, int expected) {
    assertEquals(expected, calculator.modulo(dividend, divisor));
}

@ParameterizedTest
@MethodSource("provideOrderStatuses")
@DisplayName("Should return correct display name for each status")
void getDisplayName_shouldReturnCorrectLabel(OrderStatus status, String expected) {
    assertEquals(expected, status.getDisplayName());
}

static Stream<Arguments> provideOrderStatuses() {
    return Stream.of(
        Arguments.of(OrderStatus.PENDING, "Pending"),
        Arguments.of(OrderStatus.SHIPPED, "Shipped"),
        Arguments.of(OrderStatus.DELIVERED, "Delivered")
    );
}
```

### Assertions

- Use the static methods from `org.junit.jupiter.api.Assertions` (e.g., `assertEquals`, `assertTrue`, `assertNotNull`)
- For more fluent and readable assertions, consider using a library like AssertJ (`assertThat(...).is...`)
- Use `assertThrows` or `assertDoesNotThrow` to test for exceptions
- Group related assertions with `assertAll` to ensure all assertions are checked before the test fails
- Use descriptive messages in assertions to provide clarity on failure

```java
@Test
@DisplayName("Should create order with all required fields")
void createOrder_shouldSetAllFields() {
    Order order = orderService.create("customer-1", List.of("item-a"), 2990);

    assertAll("order fields",
        () -> assertEquals("customer-1", order.getCustomerId(), "customer id"),
        () -> assertEquals(2, order.getItemCount(), "item count"),
        () -> assertNotNull(order.getCreatedAt(), "creation timestamp"),
        () -> assertEquals(OrderStatus.PENDING, order.getStatus(), "initial status")
    );
}

// AssertJ example
@Test
@DisplayName("Should return orders sorted by date")
void findRecentOrders_shouldReturnSortedResults() {
    List<Order> orders = orderService.findRecent(7);

    assertThat(orders)
        .isNotEmpty()
        .hasSizeLessThanOrEqualTo(50)
        .extracting(Order::getStatus)
        .contains(OrderStatus.SHIPPED);
}
```

### Mocking and Isolation

- Use a mocking framework like Mockito to create mock objects for dependencies
- Use `@Mock` and `@InjectMocks` annotations from Mockito to simplify mock creation and injection
- Use interfaces to facilitate mocking

```java
@ExtendWith(MockitoExtension.class)
class OrderServiceTest {

    @Mock
    private PaymentGateway paymentGateway;

    @Mock
    private InventoryClient inventoryClient;

    @InjectMocks
    private OrderService orderService;

    @Test
    @DisplayName("Should process payment when placing order")
    void placeOrder_shouldProcessPayment() {
        // Arrange
        when(inventoryClient.isAvailable("item-1")).thenReturn(true);
        when(paymentGateway.charge(any(Payment.class)))
            .thenReturn(new PaymentResult("tx-123", PaymentStatus.SUCCESS));

        // Act
        Order order = orderService.placeOrder("customer-1", "item-1");

        // Assert
        assertThat(order.getTransactionId()).isEqualTo("tx-123");
        verify(paymentGateway).charge(any(Payment.class));
    }

    @Test
    @DisplayName("Should throw when inventory is insufficient")
    void placeOrder_shouldThrow_whenItemUnavailable() {
        when(inventoryClient.isAvailable("item-1")).thenReturn(false);

        assertThrows(InsufficientInventoryException.class,
            () -> orderService.placeOrder("customer-1", "item-1"));

        verify(paymentGateway, never()).charge(any());
    }
}
```

### Test Organization

- Group tests by feature or component using packages
- Use `@Tag` to categorize tests (e.g., `@Tag("fast")`, `@Tag("integration")`)
- Use `@TestMethodOrder(MethodOrderer.OrderAnnotation.class)` and `@Order` to control test execution order when strictly necessary
- Use `@Disabled` to temporarily skip a test method or class, providing a reason
- Use `@Nested` to group tests in a nested inner class for better organization and structure

```java
@DisplayName("OrderService")
class OrderServiceTest {

    @Nested
    @DisplayName("placeOrder")
    class PlaceOrder {

        @Test
        @DisplayName("Should create order successfully")
        void shouldCreateOrder() { }

        @Test
        @DisplayName("Should reject duplicate orders")
        void shouldRejectDuplicate() { }
    }

    @Nested
    @DisplayName("cancelOrder")
    class CancelOrder {

        @Test
        @DisplayName("Should cancel pending order")
        void shouldCancelPending() { }

        @Test
        @DisplayName("Should throw when order is already shipped")
        void shouldThrowWhenShipped() { }
    }
}
```

## Commands

```bash
# Run all tests
mvn test

# Run specific test class
mvn test -Dtest=CalculatorTest

# Run tests with specific tag
mvn test -Dgroups=fast

# Run tests with Gradle
gradle test

# Run specific test with Gradle
gradle test --tests CalculatorTest
```

## Resources

- [JUnit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/)
- [AssertJ Documentation](https://assertj.github.io/doc/)
- [Mockito Documentation](https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html)

## Context7 Integration

For up-to-date information on JUnit 5 and AssertJ:

1. **Resolve the libraryId** - Use `context7_resolve-library-id`:
   - `libraryName`: "junit 5", "assertj", or "mockito"
   - `query`: what you need (e.g., "parameterized tests", "assertions")

2. **Query the docs** - Use `context7_query-docs`:
   - `libraryId`: "/junit/junit5" or "/assertj/assertj"
   - `query`: your specific question

**Before writing tests**, consult Context7 to get updated examples.
