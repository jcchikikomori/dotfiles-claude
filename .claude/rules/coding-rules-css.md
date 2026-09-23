---
paths:
  - "**/*.css"
  - "**/*.{scss,sass}"
  - "**/*.less"
---

# CSS Coding Rules

The project's Stylelint config, methodology, and CLAUDE.md win over these rules. SCSS is preferred over plain CSS and
LESS when starting something new.

## CSS

- Style with classes. Never use `#id` selectors or overqualified selectors like `div.card`.
- Never use `!important`. Fix the specificity or the structure instead.
- Never set styles through `element.style.x` in JavaScript. Toggle classes, or set a CSS custom property with
  `style.setProperty('--x', value)` for truly dynamic values.
- Follow one methodology across the project. Use BEM (`.block__element--modifier`) when the project has none.
- Write mobile-first: base styles for small screens, then `min-width` media queries.
- Put design tokens (colors, spacing, radii) in custom properties or variables, not magic numbers.
- Use `rem` for font sizes.
- Never remove focus outlines (`outline: none`) without adding a visible replacement.

## SCSS

- Use `@use` and `@forward`. Never use `@import`, which Dart Sass deprecated.
- Organize new projects with the **7-1 pattern**: seven folders and one entry file.
  - `abstracts/` holds variables, mixins, functions, and placeholders, and produces no CSS output.
  - `vendors/`, `base/`, `layout/`, `components/`, `pages/`, `themes/` hold the rest.
  - `main.scss` only loads the folders, in that order: abstracts, vendors, base, layout, components, pages, themes.
- Give each folder an `_index.scss` that `@forward`s its partials, so `main.scss` loads one file per folder.
- Prefix partials with `_`. Keep one component per file, and split a file that passes ~200 lines.
- Keep `pages/` thin. Most styles belong in `components/` and `layout/`.
- Never nest deeper than 3 levels. Use BEM `&__element` and `&--modifier` to keep nesting flat.
- Use a mixin when you need arguments. Use a `%placeholder` with `@extend` for repeated static blocks.

## LESS

- Keep variables (`@color-primary`) and mixins in their own files, loaded before components.
- Write mixins with parentheses (`.flex-center()`) so they produce no CSS output on their own.
- Use guards (`when`) for conditional mixins. Never use inline JavaScript evaluation.
- Load libraries with `@import (reference)` so only the parts you use end up in the output.
- Wrap division in parentheses (`(@gutter / 2)`), because Less 4 math treats a bare `/` as literal.
- Apply the same nesting limit (3 levels) and BEM naming as SCSS.

## See Also

- `skills-md:css` for 7-1 folder examples, methodology comparison, and anti-patterns.
- `coding-rules-html.md` for markup-side styling rules (no inline `style=""`).
