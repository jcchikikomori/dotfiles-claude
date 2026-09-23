---
paths:
  - "**/*.html"
  - "**/*.erb"
  - "**/*.liquid"
---

# HTML Coding Rules

The project's existing templates, linters, and CLAUDE.md win over these rules. ARIA and WCAG rules live in
`accessibility-web.md`.

## Semantics

- Use landmark elements (`<header>`, `<nav>`, `<main>`, `<footer>`, `<article>`, `<section>`) instead of `<div>` soup.
- Use one `<h1>` per page and do not skip heading levels.
- Use `<button>` for actions and `<a href>` for navigation. Never put a click handler on a `<div>` or `<span>`.

## Document

- Include `<!doctype html>`, a `lang` attribute on `<html>`, `<meta charset="utf-8">`, and a viewport meta tag.
- Give every page a descriptive, unique `<title>`.

## Hygiene

- Put styles in external CSS files. No `style=""` attributes and no `<style>` blocks.
- Put scripts in external JS files. No inline `on*=` handlers and no inline `<script>` bodies.
- Close and nest elements validly. Keep every `id` unique on the page.

## Media and Performance

- Set `width` and `height` on every `<img>` to prevent layout shift (CLS).
- Add `loading="lazy"` to images below the fold.
- Prefer WebP or AVIF images.
- Load scripts with `defer` or `type="module"`.

## Forms

- Associate a `<label>` with every input.
- Use the correct input `type` and `autocomplete` value.

## Templates (ERB, Liquid)

- Keep output escaping on by default.
- Keep business logic out of templates. Move it to helpers, presenters, or includes.

## See Also

- `skills-md:frontend` for component, state, and performance guidance.
- `skills-md:css` for stylesheet architecture.
- `skills-md:web-accessibility` for the full WCAG 2.2 reference.
