---
paths:
  - "**/*.sql"
  - "**/*.{psql,pgsql}"
---

# PostgreSQL Coding Rules

Apply this file when the project uses PostgreSQL. Check the database config (`database.yml` adapter, the
`docker-compose.yml` image, the `DATABASE_URL` scheme) first. For another database, ignore the dialect-specific lines.
The project's migration tool, conventions, and CLAUDE.md win over these rules.

## Queries

- Use numbered placeholders (`$1`, `$2`) or the driver's binding. Never build SQL by string concatenation.
- Name the columns you need. Never use `SELECT *` in application queries.
- Write identifiers in lowercase snake_case and never quote them. Quoted mixed-case names must be quoted forever.
- Use `NOT EXISTS` instead of `NOT IN` with a subquery. `NOT IN` returns no rows when the subquery has a `NULL`.
- Use `IS DISTINCT FROM` when either side can be `NULL`.
- Use `INSERT ... ON CONFLICT (...) DO UPDATE SET col = EXCLUDED.col` for upserts.
- Use `RETURNING` instead of a follow-up `SELECT` after a write.
- Use keyset pagination (`WHERE id > $1 ORDER BY id LIMIT $2`) instead of a large `OFFSET`.

## Indexes

- Index every foreign key column. PostgreSQL does not create these indexes automatically.
- Order composite index columns with equality filters first, then range and sort columns.
- Use partial indexes for filtered subsets (`WHERE deleted_at IS NULL`, `WHERE status = 'active'`).
- Match function calls with expression indexes (`CREATE INDEX ... ON users (lower(email))`).
- Index `jsonb` columns you query with GIN.
- Check plans with `EXPLAIN (ANALYZE, BUFFERS)`. `ANALYZE` runs the query, so wrap writes in `BEGIN` ... `ROLLBACK`.

## Schema

- Use `BIGINT GENERATED ALWAYS AS IDENTITY` for new surrogate keys, not `SERIAL`. Use `uuid` when IDs must be
  generated outside the database.
- Use `timestamptz`, never `timestamp` without time zone.
- Use `text` with a `CHECK` on length instead of `varchar(n)` or `char(n)`. They perform the same.
- Use `numeric` for money. Never use `money`, `float`, or `real` for it.
- Use `jsonb`, never `json`, unless the exact input text must be kept.
- Prefer a `CHECK` constraint or lookup table over an `ENUM` type, because enum values can't be removed.
- Declare constraints in the database: `NOT NULL`, `UNIQUE`, `CHECK`, and foreign keys with an explicit `ON DELETE`.

## Migrations on Live Tables

- Set `lock_timeout` (for example `SET lock_timeout = '5s'`) before DDL, so a blocked migration fails fast.
- Create indexes with `CREATE INDEX CONCURRENTLY`. It cannot run inside a transaction, so disable the migration
  transaction (Rails: `disable_ddl_transaction!`).
- Add foreign keys and `CHECK`s on large tables as `NOT VALID`, then run `VALIDATE CONSTRAINT` in a separate step.
- Keep one logical change per migration. Use `IF NOT EXISTS` / `IF EXISTS` in raw SQL migrations.

## Security

- Connect as a least-privilege role, never as a superuser. Grant only what each role needs.
- Use row-level security (`ENABLE ROW LEVEL SECURITY` plus policies) when tenants share tables.

## See Also

- `skills-md:postgresql` for query examples and maintenance notes.
- `skills-md:mysql` or `skills-md:oracle-sql` for other databases.
