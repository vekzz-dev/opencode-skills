# Frontend Architecture Guide

Use this guide to reason about the architecture; it is not a requirement to create a separate architecture document for every project.

## Application structure

Document the boundaries that matter to the current codebase:
- application shell and global providers;
- routes and page-level views;
- feature/domain modules;
- shared UI components and design-system primitives;
- API/data-access layer;
- state management and side effects;
- utilities and cross-cutting concerns.

Prefer feature-oriented organization when it improves cohesion, but follow established repository conventions unless there is a concrete reason to change them.

## Components

For important components, clarify:
- responsibility and user-facing purpose;
- inputs/props and emitted events/callbacks;
- local versus shared state;
- accessibility semantics and keyboard behavior;
- loading, empty, error, disabled, and success states;
- reuse boundaries and whether the component is feature-specific or shared.

Avoid abstracting a component merely because it appears twice. Abstract when the shared behavior, semantics, and change cadence are genuinely aligned.

## State and data flow

Identify the source of truth for each important piece of state. Distinguish:
- ephemeral UI state (open dialog, active tab);
- form state and validation;
- server state (fetched/cached data);
- URL state (filters, pagination, selected resource where deep-linking matters);
- persistent client preferences;
- cross-feature/global state.

Specify cache invalidation, optimistic updates, race conditions, request cancellation, retry behavior, and stale-data presentation only where relevant. Do not introduce global state for data that can remain local or URL-driven.

## Routing and navigation

Define route ownership, nested layouts, protected routes, not-found behavior, unsaved-form handling, deep links, and navigation after successful or failed actions as applicable. Client route guards improve UX but do not replace server-side access control.

## API integration

Prefer a consistent API access layer rather than scattered ad hoc fetch calls. Define request/response types from the canonical contract when possible. Handle timeouts, cancellation, duplicate submissions, pagination, validation errors, authentication expiration, and network failures according to product needs.

Do not store secrets in public frontend environment variables. Treat all data and client state as user-modifiable; validate and authorize on the server.

## Forms

Specify field semantics, required/optional status, client validation, server validation, error placement, focus behavior, submit/loading state, duplicate-submit prevention, success feedback, and recovery from failure. Client validation must be mirrored by server validation where applicable.

## Authentication and authorization UX

Describe sign-in/sign-out, expired sessions, forbidden actions, role-dependent navigation, and permission-denied states. Never rely on hiding a button or guarding a route as the only enforcement of authorization.

## Architecture decisions

Record a short ADR only for consequential choices that are difficult to reverse or affect multiple contributors (for example, a major state strategy, routing model, or design-system adoption). Include context, alternatives, decision, consequences, and status. Routine choices do not need ADRs.
