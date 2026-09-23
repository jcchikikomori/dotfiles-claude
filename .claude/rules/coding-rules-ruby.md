---
paths:
  - "**/*.rb"
  - "**/*.rake"
  - "**/Gemfile"
  - "**/*.gemspec"
---

# Ruby Coding Rules

The project's RuboCop config (`.rubocop.yml`) and its CLAUDE.md win over these rules.

## Style

- Follow the repo RuboCop config and run `rubocop` on changed files before finishing.
- Use 2-space indentation.
- Use single quotes, except when a string needs interpolation or escape sequences.
- Use stabby lambdas (`->`).
- Start every file with `# frozen_string_literal: true`, except migrations and specs.

## Control Flow

- Use guard clauses (`return unless ...`) instead of nested conditionals.
- Give every `case` an `else` that raises an error or returns an explicit default.

## Design

- Avoid Feature Envy: put logic on the object that owns the data.
- Keep classes small and single-purpose. Prefer composition over inheritance.
- Use value objects for domain concepts with behaviour (money, address, date range).
- Split any method that grows past ~20 lines.

## RSpec

- Use `let` for test data instead of instance variables.
- Use `is_expected` instead of `should`.
- Compare expectations against literal values, not other method calls: `eq(1)`, not `eq(other.value)`.
- Freeze time with `ActiveSupport::TimeHelpers` (or the repo's helper) inside an `around` block.
- Test the unauthorized and invalid-input paths, not only the happy path.

## Security

- Generate tokens with `SecureRandom`.
- Never interpolate user input into SQL. Use bound parameters.
- Never pass user input to `eval`, `send`, `constantize`, or `public_send`.

## See Also

- `skills-md:ruby` for examples, RSpec patterns, and design-pattern detail.
- `skills-md:owasp` for the full security checklist.
