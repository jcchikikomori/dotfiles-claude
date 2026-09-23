# dotfiles-claude

Personal Claude Code configuration, packaged as a GNU Stow package. Part of
[jcchikikomori/.dotfiles](https://github.com/jcchikikomori/.dotfiles).

This file covers how to work **on** this repo. The global agent instructions
that ship to every machine live in `.claude/CLAUDE.md` — that file is product
content, not instructions for this repo.

## Tech Stack

- **Content:** Markdown (instructions, rules, output styles, commands), JSON (MCP and settings)
- **Tooling:** Bash (`bin/`), `jq`
- **Packaging:** GNU Stow — top-level files symlink into `$HOME`
- **Quality gates:** pre-commit (commitizen, pre-commit-hooks, black)
- **No build, no tests, no Docker Compose.** Docker is only used by some MCP servers at runtime.

## Setup

```bash
# Stow-based install (from the parent .dotfiles checkout)
stow -t "$HOME" dotfiles-claude

# Or the manual copy described in README.md
cp -r .claude ~/

# Optional: custom spinner verbs, merged into ~/.claude/settings.json
bin/install-custom-spinner-verbs

# Repo hooks
pre-commit install --hook-type pre-commit --hook-type commit-msg
pre-commit run --all-files
```

## Key Directories

- `.claude/CLAUDE.md` — global instructions stowed to `~/.claude/CLAUDE.md`
- `.claude/rules/` — path-scoped rules; `paths:` frontmatter globs decide when each loads
- `.claude/output-styles/` — custom output styles (frontmatter `description`, `keep-coding-instructions`)
- `.claude/commands/` — slash commands (`hello-world.md` is the reference example)
- `.claude/.mcp.json` — MCP server definitions; secrets come from `${ENV_VAR}` placeholders only
- `.claude/settings.local.json` — local settings (currently the output style)
- `bin/` — helper scripts that patch `~/.claude/settings.json` via `jq`
- `.remember/` — Remember plugin runtime state, never stowed and never committed

## Rules for This Repo

- **Stow hygiene:** any new top-level file that is repo metadata (docs, lint config, tooling) must get an anchored entry
  in `.stow-local-ignore` (for example `^/CLAUDE\.md$`). Otherwise stow symlinks it into `$HOME`.
- **Never commit secrets.** MCP credentials stay as `${VAR}` placeholders in `.claude/.mcp.json`. Real values go in
  `~/.profile.local` or the shell rc file.
- **Respect `.gitignore`.** Claude runtime state (`.claude/projects/`, `.claude/plugins/`, `.claude/cache/`,
  `.claude/settings.json`, `.claude/skills/`, logs) must stay untracked.
- **Keep `.claude/` files in place.** When memory-guard flags them, do not remove or stash them — they are the repo's
  content.
- **Markdown:** keep lines at 120 characters or less (MD013); relative links must resolve (MD057).
- **JSON:** `pretty-format-json` runs in pre-commit, so keep JSON formatted with 2-space indentation.
- **Commits:** Conventional Commits, enforced by the commitizen `commit-msg` hook. Scopes seen so far: `stow`,
  `spinner`, `claude`.
- **Git:** Claude drafts commit messages; the user commits and pushes.
- **Rules files:** keep each `.claude/rules/*.md` lean (~25–50 lines of checkable imperatives) and end with a
  `See Also` link to the owning `skills-md` skill. Do not repeat what `.claude/CLAUDE.md` already covers.
