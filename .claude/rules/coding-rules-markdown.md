---
paths:
  - "**/*.md"
  - "**/*.mdx"
---

# Markdown Coding Rules

The project's markdownlint (or rumdl) config and CLAUDE.md win over these rules.

## Structure

- Use one H1 per file, as the document title.
- Never skip heading levels (H2, then H3, then H4).
- Use ATX headings (`#`). Never use underline headings (`===`, `---`).
- Put a blank line before and after every heading, list, code block, and table.
- Keep YAML frontmatter intact and at the very top of the file.

## Lists

- Use `-` for unordered lists.
- Use `1.` for every item in an ordered list, and let the renderer number them.
- Indent nested lists by 2 spaces.

## Code Blocks

- Always use fenced code blocks, never indented ones.
- Always add a language tag. Use `text` for plain output and `console` for terminal sessions.

## Tables

- Match the column count in the header, the separator row, and every body row.
- Pad cells with one space: `| value |`.

## Links and Images

- Use descriptive link text. Never use "click here" or bare URLs in prose.
- Use relative links for files in the same repo, and make sure each one resolves.
- Give every image descriptive alt text.

## Prose and Whitespace

- Keep prose lines at 120 characters or less. Code blocks and tables are exempt.
- Never leave two blank lines in a row, trailing whitespace, or a missing final newline.
- Use `**bold**` for key terms and warnings, and backticks for code, paths, flags, and env vars.
- Run the repo's Markdown linter on changed files when one is configured.

## See Also

- `skills-md:markdown` for the full standard and the MCP conversion workflow.
