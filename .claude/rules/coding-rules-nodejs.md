---
paths:
  - "**/*.{js,mjs,cjs}"
  - "**/*.{ts,mts,cts}"
  - "**/*.{jsx,tsx}"
---

# JavaScript Coding Rules

The project's ESLint, Prettier, and `tsconfig.json` settings and its CLAUDE.md win over these rules.

## Style

- Follow the repo ESLint and Prettier config. Match the module type set in `package.json` (ESM or CommonJS).
- Use `const` by default and `let` when reassigning. Never use `var`.
- Use modern syntax: destructuring, spread, optional chaining (`?.`), nullish coalescing (`??`).

## TypeScript

- Do not use `any`. Use `unknown` and narrow it, or write a proper type.
- Keep `strict` mode on. Do not add `// @ts-ignore` without a comment explaining why.
- Give exported functions explicit return types.

## Async

- Use `async`/`await` instead of `.then()` chains.
- Never mix callbacks and promises in one module.
- Run independent work with `Promise.all` or `Promise.allSettled`, not sequential `await`s.
- Never leave a floating promise: `await` it, return it, or handle its rejection.
- Move CPU-heavy work off the event loop (Worker Threads or a queue).

## Errors

- Never swallow an error in an empty `catch`.
- Throw `Error` subclasses, never strings or plain objects.
- Separate operational errors (bad input, network) from programmer errors (bugs).

## Structure

- Keep modules small and single-purpose. Avoid circular imports.
- Keep route handlers thin. Delegate business logic to services.

## Security

- Validate input at the boundary with the repo's validator (for example zod or joi).
- Use parameterized queries or the ORM. Never concatenate user input into SQL.
- Read secrets from environment variables. Never hardcode them.
- Never pass user data to `eval`, `new Function`, or `innerHTML`.

## Tests

- Use the repo's test runner. Mock external I/O (database, HTTP) in unit tests.
- Compare expectations against literal values, not other function calls.

## See Also

- `skills-md:nodejs` for server-side patterns, error classes, and tooling.
- `skills-md:frontend`, `skills-md:reactjs`, and `skills-md:vuejs` for UI code.
- `skills-md:owasp` for the full security checklist.
