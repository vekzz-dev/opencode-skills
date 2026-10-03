# Query Optimization

## Execution Plans

### How to Read Plans per Dialect

| Database | Command | Key Columns |
|----------|---------|-------------|
| PostgreSQL | `EXPLAIN (ANALYZE, BUFFERS)` | `actual time`, `rows`, `loops`, `buffers` |
| MySQL | `EXPLAIN ANALYZE` (8.0.18+) | `type`, `rows`, `Extra`, `actual time` |
| MariaDB | `EXPLAIN FORMAT=JSON` | `type`, `rows`, `Extra`, `select_type` |
| SQLite | `EXPLAIN QUERY PLAN` | `detail`, `rows` (estimates) |

### What to Look For

- **Seq Scan / full table scan** on a large table — missing index
- **Nested Loop** with many iterations — may need index or different join order
- **Sort / filesort** — might need an index that covers the `ORDER BY`
- **Estimated vs actual rows** mismatch — stale statistics (`ANALYZE`)
- **`rows` estimate >> actual returned rows** — optimizer chose a bad plan

## Join Strategies

| Type | When | Risk |
|------|------|------|
| Nested Loop | Small driving table, indexed inner table | Slow if inner table has no index |
| Hash Join | Large unindexed datasets, equi-joins | Memory-heavy (hash table in RAM) |
| Merge Join | Sorted inputs, range joins | Inputs must be sorted (may add sort step) |
| Cross Join | Accidental — no join condition | Cartesian product, almost always wrong |

### Rules

- **Index FK columns** — `ON a.user_id = b.id` needs index on `a.user_id`
- **Filter before joining** — apply `WHERE` early to reduce rows
- **Join order matters** — smallest result set first (PostgreSQL is usually good at this)
- **`EXPLAIN` after every join change** — verify the plan

## Subqueries vs CTEs vs Temp Tables

| Pattern | Pros | Cons |
|---------|------|------|
| Subquery (inline) | Optimizer can flatten/inline it | Hard to read when nested |
| CTE (`WITH x AS (...)`) | Readable, reusable in same query | PostgreSQL materializes CTEs by default (pre-12); use `NOT MATERIALIZED` in PG 12+ |
| Lateral join (`LATERAL`) | Run subquery per driving row, access outer columns | Can be slow if driving set is large without index |
| Temp table | Explicit, reusable across queries, can index | Overhead of creating, writing, and cleaning up |

### Decision

```
Need to reference same subquery multiple times in one query?
  Yes → CTE (check if your DB materializes it)
Need per-row calculation referencing outer columns?
  Yes → LATERAL
Subquery is simple, used once?
  Yes → inline subquery
Need to use the result in multiple queries or debug step by step?
  Yes → temp table
```

## Keyset Pagination (Cursor-based)

Replace `OFFSET` for large pagination. `OFFSET` skips rows — the DB still reads and discards them, getting slower with each page.

### Before (bad, gets slow)

```sql
SELECT * FROM orders
ORDER BY id
LIMIT 20 OFFSET 100000;
```

### After (keyset, stays fast)

```sql
SELECT * FROM orders
WHERE id > 100000  -- last seen id from previous page
ORDER BY id
LIMIT 20;
```

### Multi-column Keyset

```sql
-- Previous page last row: (created_at = '2024-01-15', id = 42)
SELECT * FROM orders
WHERE (created_at, id) > ('2024-01-15', 42)
ORDER BY created_at, id
LIMIT 20;
```

## Common Anti-Patterns

### N+1 Queries

```sql
-- BAD: 1 query for users + N queries for orders
SELECT * FROM users;  -- 100 users
-- then for each user:
SELECT * FROM orders WHERE user_id = ?;  -- 100 queries

-- GOOD: single query with JOIN
SELECT u.*, o.*
FROM users u
LEFT JOIN orders o ON o.user_id = u.id;
```

### Correlated Subquery

```sql
-- BAD: runs subquery for every row in orders
SELECT o.*,
  (SELECT name FROM users u WHERE u.id = o.user_id) AS user_name
FROM orders o;

-- GOOD: join instead
SELECT o.*, u.name
FROM orders o
LEFT JOIN users u ON u.id = o.user_id;
```

### `SELECT *` in Production

```sql
-- BAD: reads all columns, can't use covering index
SELECT * FROM users WHERE email = 'x@y.com';

-- GOOD: reads only needed columns, may use covering index
SELECT id, name, email FROM users WHERE email = 'x@y.com';
```

### Functions on Indexed Columns

```sql
-- BAD: function prevents index usage
SELECT * FROM orders WHERE DATE(created_at) = '2024-01-15';

-- GOOD: range query uses index
SELECT * FROM orders
WHERE created_at >= '2024-01-15' AND created_at < '2024-01-16';
```

## Dialect-Specific Tips

### PostgreSQL

- **`EXPLAIN (ANALYZE, BUFFERS, TIMING)`** — gold standard
- **Parallel query** — PG automatically parallelizes seq scans, joins, aggregates (check `max_parallel_workers_per_gather`)
- **`pg_stat_statements`** — find the most time-consuming queries
- **`CLUSTER`** — physically reorder table to match an index (good for range scans)
- **`BRIN` indexes** — huge win for append-only tables (logs, metrics)

### MySQL / MariaDB

- **`EXPLAIN FORMAT=JSON`** — more detail than tabular, shows `cost_info` (8.0+)
- **`optimizer_trace`** — `SET optimizer_trace='enabled=on'` — full optimizer decision log
- **Index merge** — MySQL can use multiple indexes per table and intersect/union them (check `Extra` column: `Using intersect`, `Using union`)
- **`MRR` (Multi-Range Read)** — optimization for range scans on secondary keys (check `Using MRR`)
- **`batched_key_access`** — join optimization when join column is not indexed on the driven side

### SQLite

- **`ANALYZE`** — must be run manually; without it, the planner guesses
- **`EXPLAIN QUERY PLAN`** — cheap, always available
- **`INTEGER PRIMARY KEY`** — already indexed (the rowid)
- **Covering indexes** — SQLite can satisfy queries entirely from an index without touching the table
- **Query planner stability** — SQLite uses a simpler planner than PostgreSQL/MySQL; adding `OR` or changing operator order can flip the plan
