# MariaDB Reference

## Differences from MySQL

MariaDB is a fork of MySQL — most MySQL knowledge applies. These are the **key differences**.

## Data Types

- `JSON` is an alias for `LONGTEXT` with a JSON validation constraint (not a native binary type like MySQL/PostgreSQL)
- `ROW` — composite type for stored procedures (not for table columns)
- `INET6` — native IPv6 address type (MySQL lacks this)
- Sequences: `CREATE SEQUENCE` — independent schema object (MySQL lacks this)
- `AUTO_INCREMENT` works like MySQL; MariaDB also supports sequences

## Storage Engines

| Engine | Use Case |
|--------|----------|
| **InnoDB** (default) | Transactional, same as MySQL |
| **Aria** | MySQL's MyISAM replacement — crash-safe, no transactions |
| **TokuDB** | Compression, fractal trees — deprecated, check version |
| **ColumnStore** | Analytics, columnar storage |
| **Spider** | Sharding, distributed queries |
| **S3** | Store tables in S3 |

## Unique Features

- **`WITH` (CTE) recursion** works, but MariaDB does **not** support `WITH ... AS NOT MATERIALIZED`
- **`EXPLAIN`** shows more details than MySQL; `EXPLAIN FORMAT=JSON` is richer
- **Thread pool** by default — handles more concurrent connections than MySQL
- **`LIMIT` inside subqueries** does not force a derived table (MariaDB is more permissive)
- **`HAVING`** can reference aliases `HAVING total > 10` without `GROUP BY` in some cases
- **`SEQUENCE` plugin**: `SELECT seq FROM seq_1_to_100` — generate numbers without a table
- **`DELETE ... RETURNING`** — MariaDB 10.0+, MySQL lacks this
- **`REPLACE` and `LOAD DATA`** have more optimization flags than MySQL

## Indexing

- Invisible indexes: `ALTER TABLE ... ALTER INDEX idx_name INVISIBLE` — test drops safely
- No `FULLTEXT` with InnoDB before MariaDB 10.0 (fine in current versions)
- `SPIDER` engine supports distributed partitioned indexes
- MariaDB supports `ALTER TABLE ... DROP INDEX ... ALGORITHM=INPLACE LOCK=NONE`

## Migration Quirks

- Same DDL as MySQL — `ALTER TABLE` copies by default but improves with each release
- `ALTER TABLE ... ADD COLUMN ... ALGORITHM=INSTANT` — MariaDB 10.3+, no table rebuild
- `ALTER TABLE ... DROP COLUMN ... ALGORITHM=INSTANT` — MariaDB 10.4+
- Transactional DDL in MariaDB 10.4+ for some operations (improvement over MySQL)

## Performance

- Thread pool: more efficient than MySQL's thread-per-connection for >200 connections
- `optimizer_switch` — same as MySQL, but defaults differ
- `aria_pagecache_buffer_size` — equivalent to MyISAM key buffer
- `max_statement_time` — kill queries that run too long (like `MAX_EXECUTION_TIME` in MySQL 8)

## Testing

```bash
# Spin up test instance
docker run -d --name mariadb-test -e MARIADB_ROOT_PASSWORD=test -p 3306:3306 mariadb:11

# Dump schema
mariadb-dump --no-data -h localhost -u root -p mydb > schema.sql

# Restore
mariadb -h localhost -u root -p mydb < dump.sql
```

## Resources

- [MariaDB Docs](https://mariadb.com/docs/)
- [MariaDB Knowledge Base](https://mariadb.com/kb/en/)
- [Differences from MySQL](https://mariadb.com/kb/en/mariadb-vs-mysql-features/)
