---
name: web-mvc
description: >
  Server-side web UI with Spring Boot: Thymeleaf templates, HTMX for dynamic interactions, Alpine.js for client-side behavior. No React, no webpack.
  Trigger: Thymeleaf, HTMX, Alpine.js, Spring MVC template, server-side rendering, web UI.
license: Apache-2.0
metadata:
  author: vekzz-dev
  version: "1.0"
---

## When to Use

- Building a web UI with Spring Boot (no frontend framework)
- Replacing jQuery or vanilla JS with declarative HTML
- Adding dynamic behavior without writing JavaScript
- Rendering templates with Thymeleaf

## Instructions

### Stack

| Tool | Role | Why |
|------|------|-----|
| **Thymeleaf** | Server-side template engine | Standard in Spring Boot, fragments, i18n, Spring Security integration |
| **HTMX** | Dynamic HTML via attributes | Click/ submit/ change → backend returns HTML fragment, no JS |
| **Alpine.js** | Lightweight client state | Toggles, dropdowns, counters, validation hints — the 5% HTMX doesn't cover |

### Thymeleaf — Quick Reference (Spring Boot 3+)

```xml
<!-- Layout via fragments -->
<div th:replace="~{fragments/header :: header}"></div>

<!-- Iteration -->
<tr th:each="user : ${users}" th:class="${user.active} ? 'active' : ''">
  <td th:text="${user.name}">Name</td>
  <td th:text="${#dates.format(user.createdAt, 'dd/MM/yyyy')}">Date</td>
</tr>

<!-- Conditionals -->
<div th:if="${#lists.isEmpty(users)}">
  <p>No users found.</p>
</div>

<!-- URL building -->
<a th:href="@{/users/{id}(id=${user.id})}">View</a>

<!-- Security (Spring Security) -->
<div sec:authorize="hasRole('ADMIN')">
  <a th:href="@{/admin}">Admin Panel</a>
</div>
<div sec:isAuthenticated()>
  <p th:text="${#authentication.name}">User</p>
</div>
```

**Don't use** `th:inline="javascript"` — it's a script injection risk.

### HTMX — Core Patterns

```xml
<!-- Click to load content -->
<button hx-get="/users/42/details" hx-target="#details">
  Load Details
</button>
<div id="details"></div>

<!-- Form submit returns HTML fragment -->
<form hx-post="/users" hx-target="#user-list" hx-swap="afterbegin">
  <input type="text" name="name" required>
  <button type="submit">Create</button>
</form>

<!-- Inline delete with confirmation -->
<button hx-delete="/users/42" hx-confirm="Delete this user?"
        hx-target="closest tr" hx-swap="outerHTML">
  Delete
</button>

<!-- Search with debounce -->
<input type="text" name="q" hx-get="/users/search"
       hx-trigger="keyup changed delay:300ms" hx-target="#results">

<!-- Polling every 30s -->
<div hx-get="/notifications" hx-trigger="every 30s" hx-swap="innerHTML">
</div>
```

**HTMX + Spring Boot**: controller returns fragments — no `@ResponseBody`, just `Model` + template name returning HTML.

```java
@PostMapping("/users")
public String createUser(@Valid User user, Model model) {
    userService.save(user);
    model.addAttribute("user", user);
    return "fragments/user-row :: user-row";
}
```

### Alpine.js — When HTMX isn't Enough

```xml
<!-- Toggle (use Alpine, not a full HTMX roundtrip) -->
<div x-data="{ open: false }">
  <button @click="open = !open">Toggle</button>
  <div x-show="open" x-transition>Hidden content</div>
</div>

<!-- Dropdown -->
<div x-data="{ selected: '' }">
  <select x-model="selected">
    <option value="">All</option>
    <option value="active">Active</option>
  </select>
  <p x-text="selected ? 'Filtering: ' + selected : 'No filter'"></p>
</div>

<!-- HTMX + Alpine: trigger after HTMX swap -->
<button hx-get="/users/42" hx-target="#detail"
        @htmx:after-request="$refs.detail.classList.remove('hidden')">
  Load
</button>
<div id="detail" x-ref="detail" class="hidden"></div>
```

### Project Setup

```xml
<!-- Thymeleaf (comes with spring-boot-starter-web) -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
<!-- Spring Security integration -->
<dependency>
    <groupId>org.thymeleaf.extras</groupId>
    <artifactId>thymeleaf-extras-springsecurity6</artifactId>
</dependency>
```

```html
<!-- layout.html — base template -->
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org"
      xmlns:sec="http://www.thymeleaf.org/extras/spring-security">
<head>
  <meta charset="UTF-8">
  <title th:text="${title}">App</title>
  <script src="https://unpkg.com/htmx.org@2"></script>
  <script defer src="https://cdn.jsdelivr.net/npm/alpinejs@3/dist/cdn.min.js"></script>
  <link rel="stylesheet" href="/css/app.css">
</head>
<body>
  <div th:replace="~{fragments/header :: header}"></div>
  <main th:replace="~{:: main}">
  <div th:replace="~{fragments/footer :: footer}"></div>
</body>
</html>
```

## Decision Table

| Need | Tool |
|------|------|
| Render a list of items from the server | Thymeleaf `th:each` |
| Submit form without page reload | HTMX `hx-post` |
| Confirm before delete | HTMX `hx-confirm` |
| Real-time search with debounce | HTMX `hx-trigger="keyup delay:300ms"` |
| Open/close a dropdown | Alpine `x-show` |
| Infinite scroll | HTMX `hx-trigger="revealed"` |
| Lazy-load a tab content | HTMX `hx-get` on click |
| Sortable table columns | HTMX `hx-get` with sort param + Alpine for arrow icon toggle |
| Client-side validation hints | Alpine `x-model` + `x-text` for error messages |

## Anti-Patterns

- **Mixing HTMX target and Alpine in the same element** — keep concerns separate
- **Using HTMX for toggles** — Alpine `x-show` is instant, HTMX needs a roundtrip
- **Returning JSON from HTMX endpoints** — HTMX expects HTML fragments, not JSON
- **Thymeleaf `th:inline="javascript"`** — XSS vector, use `data-*` attributes instead
- **Large HTML fragments** — keep fragments focused; if it's big, lazy-load with HTMX
