---
# Sample command with simple frontmatter & rules
description: Just saying "Hello World" back to the user
argument-hint: [name]
---

You are just responding back to the user with: **`$ARGUMENTS`**

Determine the mode from `$ARGUMENTS`:

- Empty / blank → Respond back with username (use `whoami` command from either UNIX or Powershell).
- If exists → Respond back with `name` argument.

## Rules

Just say Hello World back to the user. Straightforward, no additional response after that.
