---
paths:
  - "**/*.html"
  - "**/*.{erb,haml}"
  - "**/*.liquid"
  - "**/*.{jsx,tsx}"
  - "**/*.vue"
---

# Web Accessibility Rules

Target **WCAG 2.2 AA**. Use AAA where it is practical. Markup semantics live in `coding-rules-html.md`.

- Make every interactive element reachable by keyboard, with no keyboard traps.
- Keep a visible focus indicator on every focusable element, and a logical tab order.
- Add a skip link to the main content (`<a href="#main-content">`).
- Give every image `alt` text. Use `alt=""` for decorative images.
- Keep content usable at 200% text zoom.
- Respect `prefers-reduced-motion` for animation and auto-play.

## ARIA Coding Rules

- Use native HTML elements before ARIA roles. A wrong role is worse than no role.
- Author against ARIA 1.2 and the ARIA Authoring Practices Guide (APG) patterns.
- Give every custom control an accessible name (visible label, `aria-label`, or `aria-labelledby`).
- Never set `aria-hidden="true"` on focusable content or its ancestors.
- Never use a positive `tabindex`. Use `0` or `-1` only.
- Announce dynamic updates with an `aria-live` region.
- Link error messages to their fields with `aria-describedby`, and set `aria-invalid` on invalid fields.

## WCAG Compliance Rules

- Keep contrast at 4.5:1 or more for text and 3:1 or more for UI components.
- Make targets at least 24×24 CSS px (2.5.8).
- Do not let sticky headers or footers hide the focused element (2.4.11).
- Give every drag interaction a single-pointer alternative (2.5.7).
- Do not require a cognitive test to log in. Allow paste and password managers (3.3.8).
- Do not ask for information the user already entered in the same flow (3.3.7).
- Do not report 4.1.1 Parsing. WCAG 2.2 removed it.
- Run axe or Lighthouse on UI changes. They catch only about a third of issues, so also test keyboard use by hand.

## See Also

- `skills-md:web-accessibility` for full criteria, test protocols, and implementation patterns.
