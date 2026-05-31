---
name: database-design
description: >
  General database design principles: modeling, normalization, indexing, naming conventions, migrations, and query optimization.
  Trigger: Database design, data modeling, schema design, table design, or migration planning.
license: Apache-2.0
metadata:
  author: vekzz-dev
  version: "1.1"
---

## When to Use

- Designing a new database schema
- Reviewing or refactoring existing schemas
- Planning migrations or data model changes
- Choosing data types, indexes, or constraints
- Defining naming conventions for tables, columns, or relationships

## Instructions

### 1. Understand the Domain First

Before writing any DDL, map the **business entities** and their **relationships**. Use the conceptual → logical → physical flow:

1. **Conceptual**: Entities, relationships, business rules (ER diagram)
2. **Logical**: Attributes, keys, normal forms (independent of technology)
3. **Physical**: Data types, indexes, partitions, engine-specific features

### 2. Naming Conventions

| Element | Convention | Example |
|---------|-----------|---------|
| Tables | `snake_case`, plural | `users`, `order_items` |
| Columns | `snake_case`, singular | `created_at`, `email` |
| PK | `id` | `id BIGSERIAL PRIMARY KEY` |
| FK | `{singular_table}_id` | `user_id`, `order_id` |
| Join table | `{table1}_{table2}` | `users_roles` |
| Indexes | `idx_{table}_{column(s)}` | `idx_users_email` |
| Unique constraint | `uq_{table}_{column(s)}` | `uq_users_email` |

### 3. Normalization Quick Reference

- **1NF**: Atomic columns, no repeating groups
- **2NF**: 1NF + all non-key columns depend on the *whole* PK
- **3NF**: 2NF + no transitive dependencies (non-key → non-key)
- **Denormalize only** when query performance demands it, and *document why*

### 4. Data Type Selection

- Use the **smallest type** that fits the data range
- Prefer standardized types: `TIMESTAMPTZ` over `TIMESTAMP`, `UUID` or `IDENTITY` over natural keys
- Avoid `TEXT`/`VARCHAR(255)` as a default — size columns appropriately
- Use `DECIMAL` for money, not `FLOAT`
- **Dialect matters** — see [references/postgresql.md](references/postgresql.md) for pg-specific types (arrays, JSONB, intervals), [references/mysql.md](references/mysql.md) / [references/mariadb.md](references/mariadb.md) for engine quirks, and [references/sqlite.md](references/sqlite.md) for type affinity rules

### 5. Indexing Strategy

1. **PKs and FKs** — always index FK columns
2. **Query-driven** — index columns in `WHERE`, `JOIN`, `ORDER BY`, not arbitrary columns
3. **Composite indexes** — leftmost prefix rule applies; order by selectivity
4. **Covering indexes** — include columns to avoid table heap lookups
5. **Avoid over-indexing** — every index slows writes

### 6. Constraints Over Code

Prefer database constraints over application-level validation:

```
CHECK (price > 0)           -- domain integrity
NOT NULL                    -- required fields
UNIQUE                      -- uniqueness
FOREIGN KEY ... ON DELETE   -- referential integrity
```

### 7. Migration Patterns

- Every migration must be **reversible** (have a `down`/`rollback`)
- One migration = one logical change (add column, create table, etc.)
- Never modify a migration that has already been applied to production
- Use **timestamp-based** or **sequential** naming: `20250531_create_users.sql`

### 8. Query Design

- **Avoid `SELECT *`** — always name columns
- Use **EXPLAIN ANALYZE** before optimizing
- Prefer **set-based** operations over row-by-row (cursors)
- Use `LIMIT` / `OFFSET` for pagination, but prefer keyset pagination for large datasets

### 9. Decision Table

| Need | Approach |
|------|----------|
| Simple CRUD | Normalized relational model |
| Reporting / analytics | Star schema (fact + dimension tables) |
| Time-series data | Partition by time, consider specialized DB |
| Full-text search | Dedicated index (GIN, or external search) |
| Hierarchical data | Adjacency list, nested sets, or LTREE |
| Multi-tenant | Row-level security or tenant discriminator column |

## Commands

```bash
# Generate migration
migrate create -ext sql -dir migrations create_users

# Run migrations
migrate -database "$DATABASE_URL" up

# Rollback last migration
migrate -database "$DATABASE_URL" down 1

# Review dry-run
migrate -database "$DATABASE_URL" up 1 --dry-run
```

## Dialect References

| Database | File | Key Differentiators |
|----------|------|--------------------|
| PostgreSQL | [references/postgresql.md](references/postgresql.md) | Arrays, JSONB, GIN/GIST, CTEs, window functions, extensions, MVCC |
| MySQL | [references/mysql.md](references/mysql.md) | Storage engines, `AUTO_INCREMENT`, locking quirks, `EXPLAIN` variants |
| MariaDB | [references/mariadb.md](references/mariadb.md) | Sequences, `WITH` optimizations, JSON vs LONGTEXT, thread pool |
| SQLite | [references/sqlite.md](references/sqlite.md) | Type affinity, no `ALTER COLUMN`, WAL mode, embedded constraints |

### Choose Your Database

| Requirement | Recommended DB |
|-------------|---------------|
| Complex queries, analytics, JSON | PostgreSQL |
| High-traffic web app, read-heavy | MySQL / MariaDB |
| Embedded, mobile, testing, local | SQLite |
| Need MariaDB-specific features (sequences, thread pool) | MariaDB |
| Full ACID with advanced indexing | PostgreSQL |
| Minimum ops overhead | SQLite (embedded) or managed MySQL/PostgreSQL |

## Anti-Patterns

- **EAV (Entity-Attribute-Value)** — almost never the right choice; prefer JSONB or separate tables
- **Polymorphic associations** — breaks referential integrity; use concrete join tables
- **Natural keys as PK** — they change; use surrogate keys
- **One table for everything** — violates 1NF; split by entity
- **Indexes on every column** — write performance degrades
