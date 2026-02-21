# logbook

A Claude Code plugin that archives sessions to an [Obsidian](https://obsidian.md) vault.

**Local-first. No cloud sync required.** Notes are written directly to your local Obsidian vault — no account, no subscription, no data leaving your machine.

## Skills

| Skill | Description |
|---|---|
| `/logbook:save` | Checkpoint the current session to Obsidian and keep going. |
| `/logbook:done` | Archive the current session to Obsidian and terminate. |
| `/logbook:resume <title>` | Load a previously saved note as context to pick up where you left off. |
| `/logbook:list` | List recently saved sessions in the vault. |

## Requirements

This plugin requires the **mcp-obsidian** MCP server to read and write notes in your vault.

## Setup

### 1. Install mcp-obsidian

Run this command, replacing the path with your actual Obsidian vault location:

```bash
claude mcp add-json obsidian --scope user '{"type":"stdio","command":"npx","args":["@mauricio.wolff/mcp-obsidian@latest","/path/to/your/vault"]}'
```

> For full instructions and other platforms, see [mcp-obsidian.org/install](https://mcp-obsidian.org/install/).

### 2. Set `LOGBOOK_VAULT_PATH`

The bundled `.mcp.json` uses `${LOGBOOK_VAULT_PATH}` so the plugin can also start the MCP server automatically. Export the variable in your shell profile:

```bash
# ~/.zshrc or ~/.bashrc
export LOGBOOK_VAULT_PATH="/path/to/your/vault"
```

> **Note:** If you already configured mcp-obsidian via `claude mcp add-json` in step 1, the `LOGBOOK_VAULT_PATH` env var is optional — your user-scoped MCP config takes precedence.

### 3. Install the plugin

Add this repo as a marketplace, then install:

```bash
claude plugin marketplace add wobsoriano/logbook
claude plugin install logbook@logbook
```

Or load it locally during development:

```bash
claude --plugin-dir /path/to/logbook
```

## Usage

### Save a checkpoint mid-session

Run at any point to snapshot progress without ending the session. Calling it multiple times in the same session overwrites the previous checkpoint rather than creating duplicates.

```
/logbook:save
```

### Archive and exit

Run at the end of a session to save a final note and terminate Claude Code. Also appends a one-line entry to today's daily note (`YYYY-MM-DD.md`) in your vault.

```
/logbook:done
```

### Resume a previous session

Run at the start of a new session to load a saved note as context. Claude will summarise what was done and suggest the next step.

```
/logbook:resume auth-refactor
```

### List recent sessions

Browse recently saved notes without opening Obsidian.

```
/logbook:list
```

Output example:

```
 Title                  Project          Branch      Date
 ─────────────────────────────────────────────────────────
 auth-refactor          my-app           main        2026-02-21
 todo-api-setup         my-app           feat/api    2026-02-20
```

## Session chaining

When you `/logbook:resume` a note and later run `/logbook:save` or `/logbook:done`, the new note automatically links back to the one you resumed from — creating a chain you can follow across multi-session projects.

Frontmatter:
```yaml
continued-from: auth-refactor
```

Note body:
```markdown
> Continued from [[auth-refactor]]
```

## Note format

Each saved note includes YAML frontmatter and these sections:

```markdown
---
session_id: <session id>
branch: <git branch>
project: <directory name>
date: YYYY-MM-DD
continued-from: <previous note>  # only if /logbook:resume was used
---

# <title>

> Continued from [[previous-note]]  # only if /logbook:resume was used

## Summary
## Key Decisions
## What Was Done
## Open Questions / Follow-ups
## Context for Next Session
## Diagram  # only if relevant
```

## Daily notes

Every `/logbook:done` appends a one-liner to today's daily note (`YYYY-MM-DD.md`):

```
- [[auth-refactor]] · my-app · main
```

This integrates with Obsidian's daily notes workflow, giving you a running log of Claude Code sessions alongside your other daily notes.

## License

MIT
