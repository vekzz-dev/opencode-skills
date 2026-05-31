# MySQL Reference

## Storage Engines

| Engine | Use Case |
|--------|----------|
| **InnoDB** (default) | ACID, transactions, FK, row-level locking — use for everything |
| MyISAM | Table-level locking, no transactions — legacy, avoid |
| Memory (HEAP) | Temp tables, caching — data lost on restart |
| CSV | Export/import — not for production |

## Data Types

- `INT` / `BIGINT` — `UNSIGNED` for positive-only values (doubles the range)
- `VARCHAR(n)` — max 65,535 bytes per row (all columns)
- `TEXT` / `BLOB` — 65KB; `MEDIUMTEXT` / `MEDIUMBLOB` — 16MB; `LONGTEXT` / `LONGBLOB` — 4GB
- `DATETIME` — range `1000-01-01` to `9999-12-31` (no timezone)
- `TIMESTAMP` — range `1970-01-01` to `2038-01-19` (UTC conversion)
- `JSON` — native JSON type (MySQL 5.7+), validated on insert
- `ENUM` — constrained string; adding values requires `ALTER TABLE` — prefer reference tables
- `AUTO_INCREMENT` — integer primary key generator; resets on restart depending on engine

## Indexing

- `BTREE` is the only index type for InnoDB
- `FULLTEXT` indexes — MyISAM and InnoDB (5.6+)
- `SPATIAL` indexes — GIS columns (MyISAM, InnoDB 5.7+)
- Prefix indexes: `INDEX (col(10))` — index first N chars of a string
- **Index hints**: `USE INDEX`, `FORCE INDEX` — use sparingly, optimizer is usually right
- InnoDB secondary indexes include the PK implicitly

## Query Features

- `EXPLAIN` output: `type` column (const > eq_ref > ref > range > index > ALL)
- `EXPLAIN ANALYZE` — MySQL 8.0.18+, shows actual execution time
- `INSERT ... ON DUPLICATE KEY UPDATE` — upsert
- `REPLACE INTO` — `DELETE` + `INSERT` (not a real upsert; increments auto-increment)
- No CTEs before 8.0 (use derived tables); MySQL 8.0+ supports CTEs and window functions
- `GROUP BY` implicit sorting removed in 8.0 — add `ORDER BY NULL` for 5.7 compatibility

## Migration Quirks

- `ALTER TABLE` in MySQL usually copies the table (can be slow)
- Online DDL in InnoDB: `ALTER TABLE ... ALGORITHM=INPLACE, LOCK=NONE` (8.0 improves this)
- No transactional DDL — `ALTER TABLE` commits the current transaction
- `RENAME TABLE` is atomic (both in same statement or neither)
- `CHANGE COLUMN` / `MODIFY COLUMN` — `CHANGE` can rename, `MODIFY` cannot

## Performance

- `innodb_buffer_pool_size` — set to 70-80% of available RAM
- `innodb_log_file_size` — too small causes frequent checkpointing
- Monitor slow query log: `SET GLOBAL slow_query_log = ON`
- `SHOW PROCESSLIST` — find blocking queries
- Connection pooling: MySQL has low overhead per connection (thread-per-connection)

## Testing

```bash
# Spin up test instance
docker run -d --name mysql-test -e MYSQL_ROOT_PASSWORD=test -p 3306:3306 mysql:8

# Dump schema
mysqldump --no-data -h localhost -u root -p mydb > schema.sql

# Restore
mysql -h localhost -u root -p mydb < dump.sql
```

## Resources

- [MySQL Docs](https://dev.mysql.com/doc/)
- [MySQL Performance Blog](https://www.percona.com/blog/)
- [MySQL 8.0 Reference Manual](https://dev.mysql.com/doc/refman/8.0/en/)
