---
name: ui-components
description: >
  Pre-built UI component libraries for server-rendered HTML: Preline UI, HyperUI, Flowbite. Modals, tables, forms, navbars, dropdowns — no React, no build step.
  Trigger: UI components, component library, Preline, HyperUI, Flowbite, Tailwind CSS components, pre-built UI.
license: MIT
metadata:
  author: vekzz-dev
  version: "1.0"
---

## When to Use

- Building a web UI and need pre-made components (modals, tables, navbars, forms)
- Want a decent-looking UI without writing CSS from scratch
- Using Thymeleaf + HTMX + Alpine.js and need components that don't fight that stack

## Instructions

### Stack Fit

| Library | Components | JS | HTMX | Alpine | License |
|---------|-----------|-----|------|--------|---------|
| **Preline UI** | 640+ (modals, datatables, forms, nav, datepicker) | Headless (replaceable) | ✅ Perfect | ✅ Drop-in | MIT |
| **HyperUI + HyperUX** | ~200 + 9 Alpine patterns | None / Alpine-native | ✅ Perfect | ✅ Built with Alpine | MIT |
| **Flowbite** | 56+ categories, charts, datatables | flowbite.js required | ⚠️ Needs reinit after swap | ⚠️ Adaptable | MIT |

**Recommendation**: Start with **Preline UI** — best component coverage and zero friction with HTMX + Alpine.

### Preline UI — Integration

**Setup:**

```html
<!-- In your Thymeleaf layout -->
<html xmlns:th="http://www.thymeleaf.org">
<head>
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Preline JS (headless — optional, replace with Alpine) -->
  <script src="https://cdn.jsdelivr.net/npm/preline/dist/preline.js" defer></script>
  <script defer src="https://cdn.jsdelivr.net/npm/alpinejs@3/dist/cdn.min.js"></script>
</head>
```

Or with npm in a Spring Boot static resources setup:

```bash
npm install preline
```

Then in your Tailwind config:

```js
module.exports = {
  content: [
    './src/main/resources/templates/**/*.html',
    './node_modules/preline/dist/*.js',
  ],
  plugins: [require('preline/plugin')],
}
```

**Example — Modal with Alpine instead of Preline JS:**

```html
<!-- Preline HTML structure + Alpine for interactivity -->
<div x-data="{ open: false }">
  <button @click="open = true" type="button"
          class="py-2 px-4 bg-blue-600 text-white rounded-lg">
    Open Modal
  </button>
  <div x-show="open" x-cloak
       class="fixed inset-0 z-50 flex items-center justify-center">
    <div class="absolute inset-0 bg-black/50" @click="open = false"></div>
    <div class="relative bg-white rounded-xl shadow-lg p-6 max-w-md w-full">
      <h3 class="text-lg font-semibold">Modal Title</h3>
      <p class="mt-2 text-gray-600">Modal content here</p>
      <button @click="open = false" class="mt-4 px-4 py-2 bg-gray-200 rounded">Close</button>
    </div>
  </div>
</div>
```

**Example — Table with HTMX:**

```html
<!-- Thymeleaf template -->
<div class="overflow-x-auto rounded-xl border">
  <table class="min-w-full divide-y divide-gray-200">
    <thead class="bg-gray-50">
      <tr>
        <th class="px-6 py-3 text-left text-sm font-medium text-gray-500">Name</th>
        <th class="px-6 py-3 text-left text-sm font-medium text-gray-500">Role</th>
        <th class="px-6 py-3 text-right text-sm font-medium text-gray-500">Actions</th>
      </tr>
    </thead>
    <tbody class="divide-y divide-gray-200" id="user-table">
      <tr th:each="user : ${users}"
          class="hover:bg-gray-50 transition"
          th:include="fragments/user-row :: row">
      </tr>
    </tbody>
  </table>
</div>

<!-- Delete button in fragment -->
<button hx-delete="@{/users/{id}(id=${user.id})}"
        hx-target="#user-table"
        hx-swap="outerHTML"
        hx-confirm="Delete this user?"
        class="text-red-600 hover:text-red-800 text-sm font-medium">
  Delete
</button>
```

### HyperUI + HyperUX — Zero Friction

HyperUI is pure HTML + Tailwind — **zero JavaScript**. Copy and paste.

```html
<!-- Alert from HyperUI (no JS) -->
<div class="rounded-xl border border-gray-100 bg-white p-4">
  <div class="flex items-start gap-4">
    <span class="text-green-600">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" />
      </svg>
    </span>
    <div class="flex-1">
      <strong class="block font-medium text-gray-900"> Changes saved </strong>
      <p class="mt-1 text-sm text-gray-700">Your profile has been updated.</p>
    </div>
  </div>
</div>
```

HyperUX is its companion — **built with Alpine.js** for interactive components:

```html
<!-- Dropdown from HyperUX -->
<div x-data="{ open: false }" class="relative">
  <button @click="open = !open"
          class="flex items-center gap-2 px-4 py-2 bg-white border rounded-lg">
    Options
    <svg x-bind:class="open && '-rotate-180'" class="w-4 h-4 transition">...</svg>
  </button>
  <div x-show="open" @click.outside="open = false"
       class="absolute right-0 mt-2 w-48 bg-white border rounded-lg shadow-lg py-1">
    <a href="#" class="block px-4 py-2 text-sm hover:bg-gray-50">Profile</a>
    <a href="#" class="block px-4 py-2 text-sm hover:bg-gray-50">Settings</a>
    <hr class="my-1">
    <a href="#" class="block px-4 py-2 text-sm text-red-600 hover:bg-red-50">Logout</a>
  </div>
</div>
```

### Flowbite — If You Already Use It

Flowbite's JS hooks into `data-modal-target`, `data-dropdown-toggle`, etc.
After an HTMX swap, you must reinitialize:

```html
<script>
  document.addEventListener('htmx:afterSwap', function() {
    initFlowbite();  // reattach event listeners after HTMX updates the DOM
  });
</script>
```

Or skip flowbite.js entirely and use Alpine for the same components:

```html
<!-- Flowbite card (no JS needed) -->
<div class="max-w-sm bg-white border border-gray-200 rounded-xl shadow-sm">
  <img class="rounded-t-xl" src="/img/photo.jpg" alt="">
  <div class="p-5">
    <h5 class="text-xl font-bold tracking-tight text-gray-900">Card title</h5>
    <p class="mt-2 text-gray-600">Card description here.</p>
    <a href="#" class="inline-flex mt-3 px-4 py-2 text-sm font-medium text-white bg-blue-700 rounded-lg hover:bg-blue-800">
      Read more
    </a>
  </div>
</div>
```

## Decision Table

| Need | Library |
|------|---------|
| Maximum component variety | Preline UI |
| Zero JS dependency, pure HTML | HyperUI |
| Alpine-native interactive components | HyperUX |
| Charts, datatables, datepicker | Preline UI |
| Quick dashboard layout | Flowbite (templates) or Preline (blocks) |

## Anti-Patterns

- **Loading multiple component libraries** — pick ONE; mixing Preline + Flowbite breaks class naming conventions
- **Adding jQuery** for interactions you can do with Alpine — 80KB for something Alpine does in 8KB
- **Using a React component library** (shadcn, MUI, Chakra) — they don't work with server-rendered HTML
- **Fighting the CDN** — use npm + Tailwind config for proper purging in production

## Commands

```bash
# Preline with npm (Spring Boot static resources)
npm install preline

# Tailwind config for Preline
npx tailwindcss init -p
```

## Resources

- [Preline UI](https://preline.co) — 640+ components
- [HyperUI](https://hyperui.dev) — Pure HTML components
- [HyperUX](https://js.hyperui.dev) — Alpine.js patterns
- [Flowbite](https://flowbite.com) — Alternative component library
