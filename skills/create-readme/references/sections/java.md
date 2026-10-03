# Java Project README Template

Use this template for Java/Maven/Gradle projects (libraries, Spring Boot, etc.).

## Structure

```markdown
# <project-name>

One-line description of what this Java project does.

[![Maven Central][maven-badge]][maven-url]
[![License][license-badge]][license-url]
[![Java Version][java-badge]][java-url]

## Why?

Explain the problem this project solves.

## Installation

### Maven

```xml
<dependency>
  <groupId>com.example</groupId>
  <artifactId>project-name</artifactId>
  <version>1.0.0</version>
</dependency>
```

### Gradle (Groovy)

```groovy
implementation 'com.example:project-name:1.0.0'
```

### Gradle (Kotlin)

```kotlin
implementation("com.example:project-name:1.0.0")
```

## Quick Start

```java
import com.example.ProjectName;

ProjectName instance = new ProjectName();
String result = instance.doSomething("input");
System.out.println(result);
```

## Usage

### Basic Example

```java
import com.example.*;
import com.example.config.*;

var config = Config.builder()
    .option1(true)
    .option2(100)
    .build();

var service = new MyService(config);
var result = service.process(input);
```

### Spring Boot

If using Spring Boot:

```java
@RestController
@RequestMapping("/api")
public class MyController {

    @GetMapping("/hello")
    public String hello(@RequestParam String name) {
        return "Hello, " + name;
    }
}
```

Run with:

```bash
./mvnw spring-boot:run
```

## API Reference

### Class: `MyClass`

Main class of the library.

#### Constructor

```java
public MyClass()
public MyClass(MyConfig config)
```

**Parameters:**
- `config` - Configuration object

#### Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| `doSomething(String input)` | `String` | Does something |
| `process(Data data)` | `Result` | Processes data |
| `close()` | `void` | Cleanup resources |

### Interface: `MyService`

```java
public interface MyService {
    Result process(Input input);
    void configure(Config config);
}
```

## Configuration

### Application Properties

`src/main/resources/application.properties`:

```properties
# Server
server.port=8080

# Application
app.name=my-app
app.feature.enabled=true
```

### YAML Configuration

`src/main/resources/application.yml`:

```yaml
server:
  port: 8080

app:
  name: my-app
  feature:
    enabled: true
```

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `SERVER_PORT` | Server port | `8080` |
| `APP_NAME` | Application name | `my-app` |

## Building

### Maven

```bash
# Build
./mvnw clean package

# Run tests
./mvnw test

# Skip tests
./mvnw clean package -DskipTests
```

### Gradle

```bash
# Build
./gradlew build

# Run tests
./gradlew test

# Skip tests
./gradlew build -x test
```

## Docker

### Build

```bash
docker build -t project-name .
```

### Run

```bash
docker run -p 8080:8080 project-name
```

## Testing

```bash
# Maven
./mvnw test

# With coverage
./mvnw test jacoco:report

# Specific test
./mvnw test -Dtest=MyServiceTest
```

### Test Example

```java
@ExtendWith(MockitoExtension.class)
class MyServiceTest {

    @Mock
    private MyRepository repository;

    @InjectMocks
    private MyService service;

    @Test
    void shouldProcessData() {
        // Given
        var input = new Input("test");

        // When
        var result = service.process(input);

        // Then
        assertThat(result).isNotNull();
        assertThat(result.getStatus()).isEqualTo("SUCCESS");
    }
}
```

## Requirements

- Java 17+
- Maven 3.8+ or Gradle 8+

## Development Setup

```bash
# Clone
git clone https://github.com/username/project-name.git
cd project-name

# Maven
./mvnw spring-boot:run

# Gradle
./gradlew bootRun
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT - see [LICENSE](LICENSE)

## Acknowledgments

- [Library](https://github.com/example) - Description
```

## Key Elements

1. **Maven/Gradle Badges** - Central repo, version
2. **Installation** - Both Maven and Gradle
3. **Java Examples** - Working code
4. **Spring Boot** - If applicable
5. **Configuration** - Properties, YAML, ENV
6. **Build Commands** - Maven + Gradle
7. **Docker** - Containerization
8. **Testing** - JUnit/Mockito examples

## Spring Boot Specific

For Spring Boot projects:

### Dependencies

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

### Common Commands

| Command | Description |
|---------|-------------|
| `./mvnw spring-boot:run` | Run app |
| `./mvnw package` | Package JAR |
| `java -jar target/app.jar` | Run JAR |

### Profiles

```bash
./mvnw spring-boot:run -Dspring-boot.run.profiles=dev
```
