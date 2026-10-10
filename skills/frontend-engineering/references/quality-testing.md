# Frontend Quality, Testing, Security, and Release Checks

## Test strategy

Choose tests based on risk and behavior, not a fixed percentage target.

- **Unit:** pure functions, formatters, validation, reducers/state transitions, and business rules implemented in the client.
- **Component:** rendering, interactions, keyboard behavior, validation, and visible states.
- **Integration:** route + data layer, forms + API errors, shared providers, and component interactions.
- **End-to-end:** high-value user journeys and critical cross-system behavior.
- **Visual regression:** stable, visually critical interfaces when screenshot maintenance is justified.
- **Accessibility:** automated checks plus manual keyboard/focus review; add assistive-technology coverage according to risk.

Test user-observable behavior rather than implementation details. Avoid brittle tests that fail on harmless refactors.

## Quality gates

Use existing repository commands when possible. Depending on stack, checks may include formatting, linting, type checking, unit/component tests, end-to-end tests, production build, dependency audit, and accessibility checks. Do not claim a check passed unless it was actually run. Report skipped checks and reasons.

## Performance

When performance matters, set measurable goals and assess:
- initial JS/CSS and asset payload;
- route-level code splitting and lazy loading;
- rendering and avoidable re-renders;
- image/font loading and responsive assets;
- network waterfalls, caching, and request duplication;
- Core Web Vitals or other agreed metrics;
- low-end devices and slow-network behavior.

Do not optimize prematurely. Establish a baseline and target before introducing complexity.

## Security and privacy

Frontend considerations include dependency risk, XSS-safe rendering, safe URL handling, CSRF implications for cookie-based sessions, secure token/session handling, avoiding secrets in bundles, privacy-conscious telemetry, and preventing sensitive data from leaking into logs or error messages. Use framework-safe APIs and established libraries; do not invent cryptography.

The frontend is not a trust boundary. Server-side authorization, input validation, and data access controls remain necessary. Avoid exposing data in client bundles, source maps, URLs, local storage, or telemetry without justification.

## Release and regression

Before release, verify critical flows, supported browsers/devices, responsive layouts, keyboard access, error recovery, environment configuration, and API compatibility. Document known gaps and rollback/feature-flag options when appropriate. Do not require a heavyweight release checklist for a low-risk prototype.
