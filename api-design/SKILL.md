---
name: api-design
description: >
  REST API design best practices: resource naming, versioning, error handling, pagination, HATEOAS, rate limiting, OpenAPI documentation.
  Trigger: API design, REST API, endpoint, OpenAPI, RESTful, API versioning, or API documentation.
license: Apache-2.0
metadata:
  author: vekzz-dev
  version: "1.0"
---

## When to Use

- Designing a new REST API
- Reviewing or refactoring existing endpoints
- Defining error responses, pagination, or versioning strategy
- Writing OpenAPI/Swagger documentation
- Implementing HATEOAS or rate limiting

## Instructions

### 1. Resource Naming

| Pattern | Example | Notes |
|---------|---------|-------|
| `GET /resources` | `GET /users` | List collection |
| `POST /resources` | `POST /users` | Create (return 201 + Location header) |
| `GET /resources/:id` | `GET /users/42` | Single resource |
| `PUT /resources/:id` | `PUT /users/42` | Full replace |
| `PATCH /resources/:id` | `PATCH /users/42` | Partial update |
| `DELETE /resources/:id` | `DELETE /users/42` | Delete |
| `GET /resources/:id/subresources` | `GET /users/42/orders` | Sub-collection |
| `POST /resources/:id/subresources` | `POST /users/42/orders` | Create sub-resource |

**Rules:**
- **Plural nouns**, not verbs: `/users` not `/getUsers`
- **Consistent casing**: `kebab-case` or `snake_case`, not mixed
- **Nest by ownership**, not by convenience: max 2 levels deep
- **Verbs only for actions** that are not CRUD: `POST /orders/42/cancel`

### 2. Versioning

| Strategy | How | Pros | Cons |
|----------|-----|------|------|
| URI path | `GET /v1/users` | Explicit, cacheable, easy to route | URL pollution on breaking changes |
| Header | `Accept: application/vnd.myapp.v1+json` | Clean URLs, content negotiation | Harder to discover, caching complexity |
| Query param | `GET /users?version=1` | Easy to test | Pollutes query space, not cache-friendly |

**Recommendation**: URI path for public APIs, header for internal/microservices.

### 3. Error Handling (RFC 9457 / application/problem+json)

```json
HTTP/1.1 422 Unprocessable Entity
Content-Type: application/problem+json

{
  "type": "https://api.example.com/errors/order/invalid-status",
  "title": "Invalid order status transition",
  "status": 422,
  "detail": "Cannot transition order 42 from 'shipped' to 'pending'.",
  "instance": "/orders/42",
  "timestamp": "2025-05-31T10:30:00Z"
}
```

**Always use** consistent error envelope. Map HTTP status codes semantically:

| Code | When |
|------|------|
| 400 | Malformed request body / validation error |
| 401 | Missing or invalid authentication |
| 403 | Authenticated but not authorized |
| 404 | Resource not found |
| 409 | Conflict (duplicate, stale version) |
| 422 | Unprocessable entity (business rule violation) |
| 429 | Rate limited |
| 500 | Unexpected server error (never leak details) |

**Never** return `5xx` for client errors or `4xx` for server faults.

### 4. Pagination

**Cursor-based (preferred for large datasets):**

```json
GET /users?cursor=eyJpZCI6NDJ9&limit=20

{
  "data": [...],
  "pagination": {
    "next": "eyJpZCI6NjJ9",
    "limit": 20
  }
}
```

**Offset-based (acceptable for small/admin datasets):**

```json
GET /users?page=1&size=20

{
  "data": [...],
  "pagination": {
    "page": 1,
    "size": 20,
    "total": 150,
    "total_pages": 8
  }
}
```

### 5. HATEOAS

Guide clients via links in responses — they don't hardcode URLs:

```json
GET /orders/42

{
  "id": 42,
  "status": "pending",
  "_links": {
    "self": { "href": "/orders/42" },
    "cancel": { "href": "/orders/42/cancel", "method": "POST" },
    "pay": { "href": "/orders/42/pay", "method": "POST" }
  }
}
```

Spring Boot: `org.springframework.boot:spring-boot-starter-hateoas`

### 6. Rate Limiting

| Strategy | How | Best for |
|----------|-----|----------|
| Token bucket | Fixed rate, burst allowed | General purpose |
| Sliding window | Rolling time window | Strict rate enforcement |
| Leaky bucket | Smooth output rate | Queue processing |

**Response headers:**
```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 42
X-RateLimit-Reset: 1717142400
```

## OpenAPI / SpringDoc

Use `springdoc-openapi` (`springdoc-openapi-starter-webmvc-ui` for Boot 3+):

```java
@Operation(summary = "List all users", description = "Returns paginated list of active users")
@ApiResponse(responseCode = "200", description = "Users found")
@ApiResponse(responseCode = "401", description = "Unauthorized")
@GetMapping
public ResponseEntity<Page<UserResponse>> listUsers(
    @Parameter(description = "Cursor for pagination") @RequestParam(required = false) String cursor
) { ... }
```

**Annotate every endpoint** — don't rely on auto-generated names.

## Anti-Patterns

- **`/api` prefix** on every route — `api.example.com` or gateway takes care of it
- **Nested resources beyond 3 levels** — `/a/1/b/2/c/3/d/4` is unreadable; use query params
- **Returning 200 with error in body** — use correct HTTP status codes
- **Leaking stack traces** in production responses — use `@ControllerAdvice` to sanitize
- **Breaking existing clients** without versioning or migration window
- **`/getAllUsers`** — REST uses HTTP methods, not verbs in the URL

## Commands

```bash
# Generate OpenAPI spec at runtime
curl localhost:8080/v3/api-docs | jq .

# OpenAPI UI
open http://localhost:8080/swagger-ui.html
```
