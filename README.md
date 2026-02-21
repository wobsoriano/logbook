# claude-vault

A Claude Code plugin that archives sessions to an [Obsidian](https://obsidian.md) vault.

Run `/claude-vault:done` at the end of any session to save a structured note with a summary, key decisions, open follow-ups, and context for resuming — then automatically terminate the session.

## Skills

| Skill | Description |
|---|---|
| `/claude-vault:done [cost]` | Archive the current session and exit. Pass the session cost optionally (e.g. `/claude-vault:done $0.042`). |

## Requirements

This plugin requires the **mcp-obsidian** MCP server to write notes to your vault.

## Setup

### 1. Install mcp-obsidian

Run this command, replacing the path with your actual Obsidian vault location:

```bash
claude mcp add-json obsidian --scope user '{"type":"stdio","command":"npx","args":["@mauricio.wolff/mcp-obsidian@latest","/path/to/your/vault"]}'
```

> For full instructions and other platforms, see [mcp-obsidian.org/install](https://mcp-obsidian.org/install/).

### 2. Set `OBSIDIAN_VAULT_PATH`

The bundled `.mcp.json` uses `${OBSIDIAN_VAULT_PATH}` so the plugin can also start the MCP server automatically. Export the variable in your shell profile:

```bash
# ~/.zshrc or ~/.bashrc
export OBSIDIAN_VAULT_PATH="/path/to/your/vault"
```

> **Note:** If you already configured mcp-obsidian via `claude mcp add-json` in step 1, the `OBSIDIAN_VAULT_PATH` env var is optional — your user-scoped MCP config takes precedence.

### 3. Install the plugin

```bash
claude plugin install claude-vault
```

Or load it locally during development:

```bash
claude --plugin-dir /path/to/claude-vault
```

## Usage

At the end of a session, run:

```
/claude-vault:done
```

Or with the session cost:

```
/claude-vault:done $0.042
```

Claude will:
1. Gather git branch and project name
2. Analyze the full conversation
3. Write a structured `.md` note to the root of your Obsidian vault
4. Print a confirmation line and terminate the session

## Note format

Each archived note includes YAML frontmatter and these sections:

```markdown
---
session_id: <session id>
branch: <git branch>
project: <directory name>
date: YYYY-MM-DD
cost: $0.042  # omitted if not provided
---

# <title>

## Summary
## Key Decisions
## What Was Done
## Open Questions / Follow-ups
## Context for Next Session
## Diagram  # only if relevant
```

## License

MIT
