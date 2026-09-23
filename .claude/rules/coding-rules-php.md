---
paths:
  - "**/*.php"
  - "**/*.phtml"
---

# PHP Coding Rules

The project's PHP-CS-Fixer or PHP_CodeSniffer config, its PHPStan/Psalm level, and its CLAUDE.md win over these rules.

## Version First

- Read the PHP version from `composer.json` (`require.php`) before writing code.
- Use only features that version supports: enums and `readonly` need 8.1, `match` and union types need 8.0.

## Standards

- Follow PSR-12 style and PSR-4 autoloading: one class per file, namespace matches the directory.
- Name classes in `StudlyCaps`, methods in `camelCase`, constants in `UPPER_CASE`.
- Start every new file with `declare(strict_types=1);`. Do not add it to legacy files without checking callers.
- Type every parameter, return value, and property.

## Modern PHP (8.x)

- Use constructor property promotion for simple constructors.
- Use `readonly` properties for value objects and DTOs.
- Use enums instead of class constants for fixed sets of values.
- Prefer `match` over `switch`, and always include a `default` arm.
- Use `??` and `?->` instead of nested `isset()` or null checks.

## Design

- Inject dependencies through the constructor. Avoid singletons and static service locators.
- Keep controllers thin. Put business logic in services and data access in repositories.
- Split any method that grows past ~20 lines.

## Errors

- Catch specific exception types. Catch `\Throwable` or `\Exception` only to re-throw or at the top-level handler.
- Never leave a `catch` block empty.
- Throw custom exception classes for domain errors.

## Security

- Use prepared statements or the ORM's bindings. Never interpolate user input into SQL.
- Escape output for its context (`htmlspecialchars()` or the template engine's auto-escaping).
- Hash passwords with `password_hash()` and check them with `password_verify()`.
- Use `random_bytes()` or `random_int()` for secrets. Never use `rand()` or `mt_rand()`.
- Regenerate the session ID on login (`session_regenerate_id(true)`).
- Never pass user input to `eval`, `unserialize`, `include`, or shell functions.

## Tests

- Use PHPUnit (or the repo's runner) with the Arrange-Act-Assert layout.
- Use data providers for parameterized cases.
- Compare expectations against literal values, not other method calls.

## See Also

- `skills-md:php` for PSR detail, design patterns, and tooling.
- `skills-md:owasp` for the full security checklist.
