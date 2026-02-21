# claude-vault

A Claude Code plugin that archives sessions to an [Obsidian](https://obsidian.md) vault.

## Skills

| Skill | Description |
|---|---|
| `/claude-vault:save` | Checkpoint the current session to Obsidian and keep going. |
| `/claude-vault:save-and-exit` | Archive the current session to Obsidian and terminate. |
| `/claude-vault:resume <title>` | Load a previously saved note as context to pick up where you left off. |

## Requirements

This plugin requires the **mcp-obsidian** MCP server to read and write notes in your vault.

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

Add this repo as a marketplace, then install:

```bash
claude plugin marketplace add wobsoriano/claude-vault
claude plugin install claude-vault@claude-vault
```

Or load it locally during development:

```bash
claude --plugin-dir /path/to/claude-vault
```

## Usage

**Checkpoint mid-session:**
```
/claude-vault:save
```

**Archive and exit at the end of a session:**
```
/claude-vault:save-and-exit
```

**Resume a previous session:**
```
/claude-vault:resume auth-refactor
```

Claude will search the vault for a note matching the title and load its context — including what was done, open follow-ups, and the suggested next step.

## Note format

Each saved note includes YAML frontmatter and these sections:

```markdown
---
session_id: <session id>
branch: <git branch>
project: <directory name>
date: YYYY-MM-DD
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
