---
paths:
  - "**/*.sql"
  - "**/*.{pks,pkb,pls,plsql,prc,fnc,trg}"
---

# Oracle SQL Coding Rules

Apply this file when the project uses Oracle. Check the database config (`database.yml` adapter, a `jdbc:oracle` URL,
the `docker-compose.yml` image) first. For another database, ignore the dialect-specific lines. The project's
migration tool, conventions, and CLAUDE.md win over these rules.

## Queries

- Use bind variables (`:user_id`) for every value. They block SQL injection and avoid a hard parse per value.
- Use ANSI joins (`JOIN ... ON`). Never use comma joins or the `(+)` outer-join syntax in new code.
- Alias every table and qualify columns with the alias. Name the columns you need, never `SELECT *`.
- Remember that Oracle treats `''` as `NULL`. Test for empty values with `IS NULL`, never `= ''`.
- Write date literals as `DATE '2026-01-31'` or `TO_DATE(..., 'YYYY-MM-DD')`. Never rely on `NLS_DATE_FORMAT`.
- Compare columns with values of the same type. Implicit conversion (a `VARCHAR2` column against a number) skips
  the index.
- Paginate with `OFFSET ... FETCH NEXT n ROWS ONLY` on 12c+. On older versions, apply `ROWNUM` outside an
  ordered subquery, because `ROWNUM` is assigned before `ORDER BY`.
- Use `MERGE` for upserts. Prefer `EXISTS` over `IN` with large subqueries.

## Indexes

- Order composite index columns with equality filters first, then range and sort columns.
- Match function calls with function-based indexes (`CREATE INDEX ... ON users (UPPER(email))`).
- Check plans with `EXPLAIN PLAN FOR ...` and `DBMS_XPLAN.DISPLAY` before shipping complex queries.

## Schema

- Use `NUMBER GENERATED ALWAYS AS IDENTITY` for new keys on 12c+. Otherwise use a sequence, not a trigger.
- Use `VARCHAR2(n CHAR)`, never `VARCHAR`. The default byte semantics cut multibyte text short.
- Before 23ai there is no `BOOLEAN` column type. Use `NUMBER(1)` with `CHECK (col IN (0, 1))`.
- Keep identifiers at 30 bytes or less when the database is older than 12.2.
- Name objects consistently: `idx_`, `uk_`, `fk_` prefixes and `<table>_seq` for sequences.

## Migrations

- Every DDL statement commits implicitly. Put each DDL change in its own step, and never mix DDL with DML you
  may need to roll back.
- In SQL*Plus or SQLcl scripts, end each PL/SQL block with `/` on its own line. Use `SET DEFINE OFF` when data
  contains `&`.

## PL/SQL

- Prefix names so they never clash with columns: `p_` parameters, `l_` locals, `g_` package globals.
- Declare variables with `%TYPE` and `%ROWTYPE`.
- Catch named exceptions (`NO_DATA_FOUND`, `DUP_VAL_ON_INDEX`). Use `WHEN OTHERS` only to log and `RAISE`.
- Use `BULK COLLECT` with `LIMIT` and `FORALL ... SAVE EXCEPTIONS` for large batches, not row-by-row loops.
- Group related procedures and functions in packages. Avoid dynamic DDL inside PL/SQL.
- Raise business errors with `RAISE_APPLICATION_ERROR(-20001, '...')`, using codes from -20000 to -20999.
- Run dynamic SQL with `EXECUTE IMMEDIATE ... USING`, and check identifiers with `DBMS_ASSERT`.
- Declare `AUTHID CURRENT_USER` (invoker rights) unless the code must run with the owner's rights.

## Transactions

- A transaction starts implicitly at the first DML statement. Some tools autocommit, so do not rely on the client.
- Lock with `SELECT ... FOR UPDATE NOWAIT`, `WAIT n`, or `SKIP LOCKED`.
- Retry on ORA-00060 (deadlock) and ORA-08177 (serialization failure).
- Use `PRAGMA AUTONOMOUS_TRANSACTION` only for logging and audit writes that must survive a rollback.

## Security

- Grant app schemas only the object privileges they need. Never grant `DBA` or `ANY` system privileges.
- Use Virtual Private Database (VPD) policies when tenants share tables.
- Hash passwords in the application layer (bcrypt or Argon2). `DBMS_CRYPTO` is for encryption, not password hashing.

## See Also

- `skills-md:oracle-sql` for naming tables and sequence examples.
- `skills-md:postgresql` or `skills-md:mysql` for other databases.
