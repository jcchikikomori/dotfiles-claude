# Claude Code Configuration

Claude Code configuration for dotfiles (stow package).

> **Part of [jcchikikomori/.dotfiles](https://github.com/jcchikikomori/.dotfiles)** — A standalone package containing [claude](https://claude.ai) configuration files that are stowed to `~/.config/claude/`.

[![License: AI-Restricted MIT](https://img.shields.io/badge/License-AI--Restricted%20MIT-yellow.svg)](LICENSE)
[![dotfiles](https://img.shields.io/badge/dotfiles-jcchikikomori-blue.svg)](https://github.com/jcchikikomori/.dotfiles)

---

## Overview

This standalone package provides my personal Claude Code configuration.

## Requirements

### Essential Dependencies

The `dotfiles-claude` script requires:

- **Claude Code CLI** — The official Claude Code command-line interface
  - Install: <https://docs.anthropic.com/en/docs/claude-code>
  - Check: `claude --version`

### Optional Dependencies

For MCP installation, you may need:

- **npx/node** — For npx-based MCPs (Figma, Chrome DevTools, MarkdownLint)
  - Install: <https://nodejs.org/>
  - Check: `node --version`

- **uvx/uv** — For uvx-based MCPs (MarkItDown)
  - Install: `curl -LsSf https://astral.sh/uv/install.sh | sh`
  - Check: `uv --version`

- **pipx** — For pipx-based MCPs (Stack Overflow, Web Forager, Atlassian, OCR)
  - Install: `python3 -m pip install --user pipx`
  - Check: `pipx --version`

- **docker** — For docker-based MCPs (SonarQube, GitHub Docker)
  - Install: <https://docs.docker.com/get-docker/>
  - Check: `docker --version`

This will show which dependencies are installed and which are missing.

## Structure

```bash
linux/claude/
├── .claude/
│   ├── skills            # Your skills
│   └── CLAUDE.md         # System-wide instructions
└── README.md
```

## Setup

Using your favorite Terminal

```bash
# Clone this project
git clone https://github.com/jcchikikomori/dotfiles-claude.git
# Go to dotfiles-claude
cd dotfiles-claude
# Then copy the .claude folder from inside into the $HOME directory
cp -r .claude ~/
```

## My go-to MCPs

**Core MCPs:**

- `context7` — Library documentation lookup (HTTP)
- `github` — GitHub operations via official GitHub MCP server (HTTP, requires `GITHUB_PERSONAL_ACCESS_TOKEN`)
- `stackoverflow-mcp` — Stack Overflow search (pipx, requires `STACK_EXCHANGE_API_KEY`)
- `web-forager` — Web search and content fetching (pipx)

**Integration MCPs:**

- `mcp-atlassian` — Jira/Confluence integration (pipx, requires Atlassian credentials)
- `figma-mcp` — Figma design files (npx, requires `FIGMA_API_KEY`)
- `sonarqube-mcp` — Code quality analysis (Docker, requires `SONARQUBE_URL` and `SONARQUBE_TOKEN`)
- `buildkite-mcp` — CI/CD pipeline integration (HTTP)

**Development MCPs:**

- `chrome-devtools` — Browser automation and testing (npx)
- `github-mcp-docker` — Docker-based GitHub alternative (Docker)
- `figma-developer-mcp` — Alternative Figma implementation (npx)

**Document Processing MCPs:**

- `markitdown-mcp` — Convert various formats (PDF, DOCX, PPTX, XLSX, images with OCR, audio with transcription, HTML, etc.) to Markdown (uvx, requires Python 3.10+)
- `markdownlint-mcp` — Lint and auto-fix Markdown files with 30/52 rules (npx)
- `mcp-ocr` — Extract text from images using Tesseract OCR (pipx)

**Note:** MCPs use different transports:

- **HTTP** — Connects to remote servers (e.g., GitHub's official MCP at `api.githubcopilot.com`)
- **stdio** — Runs local binaries (most other MCPs)

#### Environment Variable Setup

Add your MCP personal tokens to `~/.profile` or `~/.bashrc` or `~/.zshrc`:

```bash
# ~/.profile.local
export GITHUB_PERSONAL_ACCESS_TOKEN="ghp_xxxxxxxxxxxxxxxxxxxx"
export STACK_EXCHANGE_API_KEY="your_key_here"
export FIGMA_API_KEY="figd_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Atlassian (optional)
export JIRA_URL="https://your-domain.atlassian.net"
export JIRA_USERNAME="your.email@example.com"
export JIRA_API_TOKEN="your_token_here"
```
