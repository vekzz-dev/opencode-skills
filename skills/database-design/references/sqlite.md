# SQLite Reference

## Key Constraints

SQLite is an embedded database. It is **not** a client-server DB. Use it for:

- Local/mobile apps (Android, iOS, desktop)
- Testing (fast, zero-config, in-memory mode)
- Prototyping and dev environments
- Batch/single-user scenarios

**Avoid** for: concurrent writes from many processes, high-volume multi-user apps, replication, stored procedures.

## Type Affinity (NOT Types)

SQLite does **not** enforce column types. It uses **type affinity** — a recommended type that the column prefers, but any value can be stored in any column:

| Affinity | Rule | Example |
|----------|------|---------|
| `TEXT` | `CHAR`, `CLOB`, `TEXT`, `VARCHAR` | `VARCHAR(255)` → TEXT affinity |
| `NUMERIC` | `NUMERIC`, `DECIMAL`, `BOOLEAN`, `DATE` | `BOOLEAN` → stored as 0/1 |
| `INTEGER` | `INT`, `BIGINT`, `SMALLINT` | `INTEGER PRIMARY KEY` → rowid alias |
| `REAL` | `REAL`, `FLOAT`, `DOUBLE` | `FLOAT` → 8-byte IEEE |
| `BLOB` | No keywords | Stored as-is |

`INTEGER PRIMARY KEY` becomes an alias for the internal `rowid` — always use this for single-column PKs.

## Data Types

- No `BOOLEAN` — use 0/1; `TRUE`/`FALSE` are literals that convert to 1/0
- No `DATETIME` — use `TEXT` (ISO 8601), `INTEGER` (Unix epoch), or `REAL` (Julian day)
- No `UUID` type — store as `TEXT` (36 chars) or `BLOB` (16 bytes)
- No `JSON` type — `JSON` functions validate/extract but column is `TEXT`
- `AUTOINCREMENT` is optional and adds overhead — `INTEGER PRIMARY KEY` auto-increments by default

## SQL Quirks

- `ALTER TABLE` is limited: can only `RENAME COLUMN` (3.25+), `ADD COLUMN`, `RENAME TO`
- **No `ALTER COLUMN ... SET TYPE`** or **`DROP COLUMN`** — must recreate the table
- Schema changes in transactions are allowed (since 3.25+)
- Foreign keys are **not enforced** by default — enable with `PRAGMA foreign_keys = ON`

## Indexing

- `CREATE INDEX` — BTREE only
- Partial indexes — `CREATE INDEX idx_active ON users(is_active) WHERE is_active = 1`
- `EXPLAIN QUERY PLAN` — shows index usage (cheaper than EXPLAIN in other DBs)
- `ANALYZE` collects statistics for the query planner
- `INTEGER PRIMARY KEY` is already indexed (the rowid)

## Performance

- **WAL mode**: `PRAGMA journal_mode=WAL` — allows concurrent reads + writes; enables `NORMAL` fsync
- **Synchronous**: `PRAGMA synchronous=NORMAL` — 10-100x faster writes, safe with WAL
- **Page size**: `PRAGMA page_size=4096` or 8192 — set at database creation
- **Cache**: `PRAGMA cache_size=-64000` — 64MB cache
- **Batch inserts**: wrap in `BEGIN`/`COMMIT`; each INSERT outside a transaction is its own transaction
- **`INSERT`**: Use `INSERT INTO t VALUES (1), (2), (3)` instead of individual inserts

### Pragmas for Production

```sql
PRAGMA journal_mode = WAL;
PRAGMA synchronous = NORMAL;
PRAGMA foreign_keys = ON;
PRAGMA busy_timeout = 5000;
```

## Migration Strategy

Since SQLite lacks `ALTER COLUMN` / `DROP COLUMN`, use this pattern for complex migrations:

```sql
-- 1. Create new table with desired schema
CREATE TABLE users_new (
    id INTEGER PRIMARY KEY,
    email TEXT NOT NULL UNIQUE,
    name TEXT NOT NULL
);

-- 2. Copy data with transformation
INSERT INTO users_new (id, email, name)
SELECT id, email, full_name FROM users;

-- 3. Drop old table
DROP TABLE users;

-- 4. Rename new table
ALTER TABLE users_new RENAME TO users;
```

Use migration tools (like `sqlite-utils` or hand-written scripts) for this pattern.

## Testing

```python
# Python in-memory test
import sqlite3
conn = sqlite3.connect(":memory:")
conn.execute("PRAGMA foreign_keys = ON")
```

```bash
# CLI
sqlite3 test.db

# Dump schema
sqlite3 test.db ".schema"

# Dump full DB
sqlite3 test.db ".dump" > dump.sql

# Create from script
sqlite3 test.db < schema.sql
```

## Resources

- [SQLite Docs](https://www.sqlite.org/docs.html)
- [SQLite Query Planning](https://www.sqlite.org/optoverview.html)
- [SQLite Pragmas](https://www.sqlite.org/pragma.html)
