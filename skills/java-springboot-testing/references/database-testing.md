# Database Testing — State, Migrations, and Engine Variants

> Covers what the container setup does not decide for you: how tests share (or reset) database state, how to test Flyway migrations properly, engine variants (PostgreSQL, MySQL/MariaDB, SQLite), and running Testcontainers on Podman. Container setup basics live in [testcontainers-jdbc.md](testcontainers-jdbc.md).

## Data state between tests

Choose **one** strategy per test suite. Mixing strategies within a suite is the main source of order-dependent, flaky persistence tests.

| Strategy | How | Best for | Risks |
|---|---|---|---|
| Transactional rollback | `@Transactional` on the test (default in `@DataJpaTest`) | Most slice tests | Code that opens its own transaction or commits separately escapes rollback; breaks under `@Async`, callbacks with `REQUIRES_NEW`, or custom `Connection` use |
| `@Sql` setup/cleanup | Annotated per-test scripts (fresh state per test) | Seed data that later steps mutate; `@SpringBootTest` suites | Slower than rollback; scripts drift from production schema if not generated from migrations |
| Drop-and-recreate schema | Recreate schema in `@BeforeAll` (PostgreSQL: drop/recreate schema or database; MySQL/MariaDB: drop/recreate database) | Suite-level reset after migration tests | Slow; do it once per class, not per test |

Rules that hold for every engine:

- Tests must pass in any order and any JVM without leftover state.
- Assert on what setup created, never on pre-existing row counts (`select count(*)` targets are a design smell).
- Prefer `@Sql` (or shared `TestDataBuilder`) over copying SQL blocks between test classes.

## Migration testing with Flyway

Flyway is the primary migration tool here. The same discipline translates to Liquibase (checksummed scripts, forward-only). What is missing from a Flyway setup recipe in [testcontainers-jdbc.md](testcontainers-jdbc.md) is the *testing discipline*:

**1. Idempotency trial (cheap, catches most breakage).** On a fresh container: migrate → assert schema and `flyway_schema_history` → migrate again → expect success with no changes applied. A migration that cannot run twice cleanly on the same database will fail in CI sooner or later.

**2. Forward-only discipline.** Production migrates forward; write no relied-upon "undo" scripts. Rollback is handled by a new, forward migration. If a migration fails, fix forward.

**3. Code must follow migrations, not the other way around.** Add the migration first, then the entity/repository that uses the migrated schema — and run the persistence slice tests against the fully migrated schema. A test suite that green-lights code whose schema comes only from `createTablesIfNotExists`-style semantics is not testing real deployments.

**4. Baseline is for existing databases.** Use `baseline-on-migrate` only for pre-existing legacy schemas; never on fresh environments, where it would silently skip migrations.

**5. Engine-feature budget.** Use only syntax and features present in **every** engine and version the project declares in its support matrix. If the matrix is "PostgreSQL 17/18, MySQL 8, MariaDB 11", a PostgreSQL-18-exclusive migration pattern cannot live in shared scripts — either split engine-specific migrations or lower the feature.

**Engine caveats for migrations:**

- **SQLite:** limited `ALTER` support — column changes require the rebuild-table pattern (new table → copy → drop → rename). Keep SQLite migrations minimal and local, and do not treat them as substitutes for real-engine migrations.
- **MySQL/MariaDB:** no transactional DDL — a partially applied migration may leave half-executed changes; also mind implicit commits from DDL.
- **PostgreSQL:** transactional DDL makes migrations safer; do not build cross-engine requirements on it.

## SQLite variant (no container)

### The H2 trap (default, invisible)

`@DataJpaTest` and other slice annotations replace the datasource with an embedded database **by default** — H2, whenever it is on the classpath. Nobody decided that; the annotation did. A suite can run entirely on H2 while the team believes it tests PostgreSQL. Details in [datajpatest.md](datajpatest.md) ("Using H2 vs Real Database"); the fix is explicitness:

```java
@DataJpaTest
@AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE) // no silent H2
```

Unique cheap legitimate use: tens-of-milliseconds mapping smoke checks in heavily constrained environments, accepting that H2's PostgreSQL mode still diverges on dialect, `ON CONFLICT`, locking, sequences, and JSON. For production parity, real engine via Testcontainers ([testcontainers-jdbc.md](testcontainers-jdbc.md)).

### SQLite without a container

SQLite runs in-memory or on-file and has no Testcontainers lifecycle. For a slice test:

```java
@DataJpaTest
@AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)
@ActiveProfiles("sqlite-test") // datasource url jdbc:sqlite::memory: + hibernate dialect
class OrderRepositorySqliteTest {
}
```

Honest limits: SQLite skips dialect parity for locking/concurrency behavior, transaction isolation edge cases, `UUID`/`JSON`/`ENUM` handling, and identifier quoting rules. Fine for fast local control tests; **do not** let it be the only engine that a critical integration path runs against — keep a Testcontainers suite on the project's primary engine.

## Multi-engine projects (parametrized)

When a project supports several engines, declare the engine matrix in its documentation and parametrize the suite over it instead of duplicating test classes:

**Engine selection rule:** if the project has not declared a matrix, do not default to the examples' engine — derive the target engine from the project's own datasource (build file dependencies, `application.yaml`/`application-*.yaml`, compose files) and record the choice; ask only when the evidence is genuinely ambiguous. The engine of the suite follows the engine of the project, never the engine of an example.

```java
// One base class; per-engine containers created by a shared test resource
abstract class OrderRepositoryContractTest {
    static DataSource containerFor(Engine engine) { /* postgres | mysql | mariadb */ }
}
```

Pin each image version in the project's build or test configuration (`postgres:18-alpine`, `mysql:8.4`, `mariadb:11.6`); never `latest`. Version bumps are project decisions, recorded with the matrix.

## Testcontainers on Podman

Testcontainers works through a Docker-compatible socket — it talks to the Docker API over a socket, not to the `docker` CLI. With Podman installed, prefer model A or B:

**A. Podman socket mounted as the Docker socket:** enable the rootless Podman socket (`podman system service` / enabled user unit) so clients reach it at `/var/run/docker.sock`, or point `DOCKER_HOST` there (typically `unix:///run/user/<uid>/podman/podman.sock`). Testcontainers then works unchanged.

**B. The `podman-docker` shim:** the shim makes the `docker` *CLI* run Podman. It does not change what Testcontainers sees — the Java client never invokes that CLI, so the socket configuration above still decides everything. The shim is valuable for shell scripts and CI steps that call `docker compose` or other docker commands; they transparently run Podman.

In both cases verify these against your Testcontainers version:

- `DOCKER_HOST` points at the rootless socket, or `~/.testcontainers.properties` carries the strategy.
- `TESTCONTAINERS_DOCKER_SOCKET_OVERRIDE` points the resource reaper (ryuk) at the in-container socket path.
- Rootless Podman may need the reaper disabled or the socket explicitly stamped; see the Testcontainers Podman documentation for the version in use.

If both Docker and Podman exist on the machine, decide explicitly which one owns the socket and document that choice in the project (README, `.testcontainers.properties`, or CI env); "the shim decides" is not a stable configuration — keep the Docker/Podman configuration consistent with the docker-compose file the project already defines for local databases.
