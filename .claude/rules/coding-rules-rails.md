---
paths:
  - "app/**/*.rb"
  - "config/**/*.rb"
  - "db/**/*.rb"
  - "lib/**/*.rb"
  - "spec/**/*.rb"
  - "app/views/**/*.erb"
---

# Ruby on Rails Coding Rules

The project's CLAUDE.md and existing code conventions win over these rules. Ruby language style lives in
`coding-rules-ruby.md`.

## Version First

- Read the Rails and Ruby versions from `Gemfile.lock` and `.ruby-version` before writing code.
- Use only APIs that exist in that version (for example `before_filter` on 4.x, `ApplicationRecord` from 5.x).
- Never suggest a Rails or Ruby upgrade unless asked.

## Architecture

- Keep controllers thin. Put business logic in service objects under `app/services/`.
- Put shared behaviour in concerns (`app/models/concerns/`, `app/controllers/concerns/`).
- Copy the structure of the nearest existing component. Do not invent a new pattern when one exists.

## Views

- Keep business logic out of views. Move it to helpers, presenters, or decorators.
- Put JavaScript in asset or pack files, never inline in views.
- Keep Rails' default escaping. Use `html_safe` or `raw` only on trusted content.

## Security

- Use Strong Parameters on every create and update.
- Never disable `protect_from_forgery`.
- Set `config.force_ssl = true` in production.
- Authorize deny-by-default: every action checks permission explicitly.

## Migrations

- Prefer `change`, then `up`/`down`. Never use `self.up`/`self.down`.
- Keep every migration reversible, or raise `ActiveRecord::IrreversibleMigration` in `down`.
- Add the index in the same migration as the column it covers.

## Specs

- Require `rails_helper` in every spec.
- Add `render_views` to controller specs, when the app uses controller specs.
- Cover unauthenticated and unauthorized requests for every controller action.

## See Also

- `skills-md:ruby-on-rails` for legacy-version notes, RSpec patterns, and debugging workflow.
- `skills-md:ruby-on-rails-migrate` for migrations that cascade across models, controllers, and views.
- `skills-md:owasp` for the full security checklist.
