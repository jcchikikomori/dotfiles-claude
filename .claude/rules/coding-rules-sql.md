---
paths:
  - "**/*.sql"
  - "**/*.{psql,pgsql}"
  - "**/*.{pks,pkb,pls,plsql,prc,fnc,trg}"
---

# SQL Transaction and Procedure Rules

These rules apply to every database. Dialect syntax lives in `coding-rules-mysql.md`, `coding-rules-postgresql.md`,
and `coding-rules-oracle-sql.md`. The project's conventions and CLAUDE.md win over these rules.

## Transactions

- Wrap statements that must succeed or fail together in one explicit transaction. A single statement is already atomic.
- End every transaction with `COMMIT` or `ROLLBACK` on every path, including errors. Never leave one open.
- Keep transactions short. Never wait on network calls, user input, or long reports while one is open.
- Lock rows before changing them with `SELECT ... FOR UPDATE`, instead of reading and then writing unlocked.
- Lock rows and tables in the same order in every code path (for example ascending primary key) to avoid deadlocks.
- Use `NOWAIT` to fail fast, and `SKIP LOCKED` for queue workers that pick up jobs.
- Keep the default isolation level unless a query needs more. Justify any use of `SERIALIZABLE` in a comment.
- Retry the whole transaction on deadlock or serialization errors, a bounded number of times with backoff.
- Make retried work idempotent, with unique keys or upserts, so a retry cannot double-apply a change.
- Use `SAVEPOINT` and `ROLLBACK TO SAVEPOINT` to undo one step without losing the whole transaction.
- Commit large data changes in batches (for example 1,000 rows), not one row at a time and not all at once.
- Check whether the database commits DDL implicitly before mixing DDL and DML in one transaction.

## Stored Procedures and Functions

- Follow the project's convention on what logic belongs in the database. Do not move app logic into procedures
  unasked.
- Give each procedure one job. Use a function only when the caller needs a return value.
- Validate parameters at the top and raise a clear, named error for bad input.
- Decide who owns the transaction. A procedure that other procedures call must not `COMMIT` or `ROLLBACK`; the
  top-level caller does.
- Prefer set-based statements over cursor loops. Use a loop only when each row needs separate logic.
- Never build dynamic SQL by concatenating input. Bind values, and quote identifiers with the dialect's helper.
- Handle errors by logging and re-raising. Never swallow an error inside a procedure.
- Run as the caller's privileges (invoker rights) unless the procedure must elevate. Then keep it minimal.
- Ship procedures through migration files (`CREATE OR REPLACE`). Never edit them by hand in production.

## See Also

- `skills-md:mysql`, `skills-md:postgresql`, and `skills-md:oracle-sql` for dialect detail.
