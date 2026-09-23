---
paths:
  - "**/*.sql"
---

# MySQL Coding Rules

Apply this file when the project uses MySQL or MariaDB. For another database, ignore the dialect-specific lines. The
project's migration tool, conventions, and CLAUDE.md win over these rules.

## Queries

- Use ANSI joins (`JOIN ... ON`). Never use comma joins in `FROM`.
- Alias every table (`FROM users AS u`) and qualify columns with the alias.
- Write keywords in UPPER_CASE and identifiers in snake_case. Put each clause on its own line.
- Use placeholders (`?`) for every user-supplied value. Never build SQL by string concatenation.
- Name the columns you need. Never use `SELECT *` in application queries.
- List the target columns in every `INSERT`.
- When a table has `deleted_at`, filter `deleted_at IS NULL` in every read query.

## Performance

- Never wrap an indexed column in a function inside `WHERE` (`DATE(created_at) = ...`). Use a range instead.
- Order composite index columns by the leftmost-prefix rule: equality filters first, then range and sort columns.
- Run `EXPLAIN` on new complex queries and fix any full table scans on large tables.
- Use keyset pagination (`WHERE id > ? ORDER BY id LIMIT ?`) instead of a large `OFFSET`.
- Prefer `EXISTS` over `IN` with large subqueries.
- Batch large `UPDATE` and `DELETE` statements with `LIMIT` to keep locks short.

## Schema

- Use InnoDB. Use `BIGINT UNSIGNED AUTO_INCREMENT` for surrogate primary keys on tables that can grow large.
- Mark mandatory columns `NOT NULL`, with a `DEFAULT` where a sensible one exists.
- Use `utf8mb4`. Never use `utf8` or `utf8mb3`.
- Use `DATETIME` for business timestamps. `TIMESTAMP` stops at 2038.
- Name objects consistently: `idx_<table>_<columns>`, `uk_<table>_<columns>`, `fk_<table>_<ref_table>`.
- Alter large tables with `ALGORITHM=INSTANT` or `INPLACE`, or an online tool (gh-ost, pt-online-schema-change).

## Transactions and Security

- Wrap multi-statement writes in `START TRANSACTION` and `COMMIT`, with `ROLLBACK` on error. Keep them short.
- Use `INSERT ... ON DUPLICATE KEY UPDATE` for upserts. On MySQL 8.0.19+, use the row alias, not `VALUES()`.
- Grant app users only the privileges they need, on specific schemas. Never grant `ALL PRIVILEGES` or `SUPER`.

## MariaDB Notes

- `FOR UPDATE SKIP LOCKED` needs MariaDB 10.6+. `SEQUENCE` objects need 10.3+.
- MariaDB's JSON functions differ from MySQL 8. Test them when switching engines.

## See Also

- `skills-md:mysql` for naming tables, soft-delete examples, and MariaDB detail.
- `skills-md:postgresql` or `skills-md:oracle-sql` for other databases.
