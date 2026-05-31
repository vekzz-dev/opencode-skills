# Project Setup

## Build Tool

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

## Package Structure

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

## Starters

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
