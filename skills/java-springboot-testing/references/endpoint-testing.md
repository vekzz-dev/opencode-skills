# Endpoint Testing Patterns — Security, Pagination, Multi-Engine

> Patterns for the borderline decisions left open by the slice and container references. Tier 1 = `@WebMvcTest` slice; Tier 2 = `@SpringBootTest(RANDOM_PORT)` full stack with a real engine container ([testcontainers-jdbc.md](testcontainers-jdbc.md), [database-testing.md](database-testing.md)). Spring Boot 4 idioms throughout: `MockMvcTester`, `@MockitoBean` (never the deprecated `@MockBean`), `RestTestClient`.

## Security / auth in endpoints

Default trap: `@WebMvcTest` auto-configures Spring Security, but **your custom `SecurityFilterChain` bean is not scanned into a slice**. An unconfigured slice tests a default chain, not yours.

**Tier 1 — your real chain, in the slice:**

```java
@WebMvcTest(OrderController.class)
@Import(SecurityConfig.class) // the real filter chain, cheaply
class OrderControllerSecurityTest {
    @Autowired MockMvcTester mvc;
    @MockitoBean OrderService orderService;

    @Test // authenticated, correct role → passes the real chain
    void userSeesOrder() {
        assertThat(mvc.get().uri("/orders/1")
            .with(jwt().authorities("ROLE_USER")))
            .hasStatusOk();
    }

    @Test // no credentials → 401 from your chain
    void unauthenticatedIsRejected() {
        assertThat(mvc.get().uri("/orders/1"))
            .hasStatus(HttpStatus.UNAUTHORIZED);
    }

    @Test // authenticated, wrong role → 403, not 404
    void wrongRoleIsForbidden() {
        assertThat(mvc.get().uri("/orders/1")
            .with(jwt().authorities("ROLE_GUEST")))
            .hasStatus(HttpStatus.FORBIDDEN);
    }
}
```

Rules:

- Assert **all three boundaries** where they exist: no credentials → 401; wrong role → 403; valid → 200. A suite that only checks the happy path hides over-permissive chains.
- `401 vs 403` are different statuses with different meanings: distinguish them; a chain collapsing both hides missing authentication.
- `@WithMockUser` (webmvctest.md) tests method security cheaply but invents a principal the real issuer would never produce; `jwt()` (from `spring-security-test`) stays closer to production claims. Prefer `jwt()` when the chain contains claim-based rules (`hasRole`, claim mapping, token audience).
- `addFilters = false` is a legitimate escape for pure routing/mapping tests — but label the class accordingly (e.g. `RouterContractTest`); it must never coexist with security assertions.

**Tier 2 — end-to-end auth over real HTTP.** `jwt()` post-processors do not travel over a socket. With `@SpringBootTest(webEnvironment = RANDOM_PORT)` + a real-engine container:

- Run a **test issuer** under the test profile (own issuer URI and signing key, injected like production config), and obtain real tokens over HTTP; exchange attacks and claim wiring get tested for real.
- Session-based auth: exercise the real login/refresh endpoints with `RestTestClient` ([resttestclient.md](resttestclient.md)) and keep a cookie/session per test scenario.
- The DB-backed checks (load-scoped data, ownership rules) belong here, against the migrated schema of [database-testing.md](database-testing.md).

**Context caching trap.** Every distinct set of annotations and `@Import`s builds a new Spring context. Do not invent per-class `@Import` combinations; define **one composed annotation** (e.g. `@WebMvcTestOf(OrderController.class)` style or a shared meta-annotation) so all Tier 1 controller tests share one cached context ([context-caching.md](context-caching.md)). `jwt()` / `@WithMockUser` are per-request and do not poison the cache.

## Pagination over HTTP

Slice-level pagination basics (repository `Page` shape) live in [datajpatest.md](datajpatest.md). At the HTTP contract tier, four rules prevent the classic flaky and brittle pagination tests:

**1. Total order, always.** Never expose only `sort=createdAt,desc` — produce the well-defined keyed rule with a unique tie-breaker: `sort=createdAt,desc,id,asc`. A page boundary without a tie-breaker is unstable the moment timestamps tie.

```java
@Test
void firstPageIsStableOrder() {
    assertThat(mvc.get().uri("/orders?page=0&size=2&sort=createdAt,desc,id,asc"))
        .hasStatusOk()
        .bodyJson().extractingPath("$.content[*].id")
        .isEqualTo(new Object[] {43, 17}); // deterministic with the @Sql seed
}
```

**2. Assert form and representatives, not snapshots.** Check `content` length, first/last ids against the `@Sql` seed, contract-quoted fields such as `totalElements`/`totalPages`, and page links only when page links are part of the contract. Full-body snapshots break on harmless payload additions; representative assertions survive them.

**3. Boundaries are named tests, not memory declarations.**

| Request | Expected |
|---|---|
| no params | documented default page/size/sort |
| `size` above max | 400 (or documented cap — decide once, test exactly what was decided) |
| invalid `sort` property | 400 from a `@ControllerAdvice`, never a 500 leaking internal types |
| page beyond the end | 200 with empty content, not an error |
| filter + sort + size combined | parametrized combination test |

**4. Parametrize combinations, not pages.** `@ParameterizedTest` over the meaningful matrix (default values / middle / last page / mixed filter+sort) beats N hand-written near-identical tests; seed via the strategies of [database-testing.md](database-testing.md).

Keep a minimal, manual Hoppscotch collection mirroring the same combinations for exploratory runs — same contract, two surfaces.

## Multi-engine endpoint suites

Full matrix for everything is a cost trap. Prioritize by SQL risk, not by symmetry:

- **Keep Tier 2 on the primary engine** (per `database-testing.md`, that is the project's own declared matrix head) — most CRUD behavior is JPA-translated and identical across engines.
- **Parametrize only SQL-sensitive endpoints** across engines: aggregations, pagination ordering, locking/optimistic behavior, native queries, engine-specific types (`JSON`, `UUID`, array types).
- **Exclude SQLite from the HTTP contract matrix** for locking/concurrency/`ALTER` parity divergence ([database-testing.md](database-testing.md)); only mapping smoke checks remain there.
- **Manage the cost:** every engine is one extra Spring context (cache key includes the connection factory). Tag the matrix run (`@Tag("matrix")`) and keep it on `mvn verify -P integration-test` or a nightly/pre-release CI stage instead of every commit.
- Apply the same **engine-feature budget** as migrations: an endpoint contract only runs on engines the project declared; endpoints are not parametrized on engines the project never ships.

When no matrix is declared, derive the engine from the project (see the engine selection rule in [database-testing.md](database-testing.md)) — the examples' PostgreSQL is illustrative, not a default.
