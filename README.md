# logbook

A Claude Code plugin that archives sessions to an [Obsidian](https://obsidian.md) vault.

**Local-first. No cloud sync required.** Notes are written directly to your local Obsidian vault — no account, no subscription, no data leaving your machine.

## Skills

| Skill | Description |
|---|---|
| `/logbook:save` | Checkpoint the current session to Obsidian and keep going. |
| `/logbook:done` | Archive the current session to Obsidian and terminate. |
| `/logbook:resume <title>` | Load a previously saved note as context to pick up where you left off. |

## Requirements

This plugin requires the **mcp-obsidian** MCP server to read and write notes in your vault.

## Setup

> **Complete these steps in order.** The plugin bundles the Obsidian MCP server and reads `LOGBOOK_VAULT_PATH` on startup — if the variable isn't set, the plugin will fail to load.

### 1. Set `LOGBOOK_VAULT_PATH`

Export your Obsidian vault path in your shell profile:

```bash
# ~/.zshrc or ~/.bashrc
export LOGBOOK_VAULT_PATH="/path/to/your/vault"
```

Then reload your shell (`source ~/.zshrc`) before continuing.

### 2. Install the plugin

Add this repo as a marketplace, then install:

```bash
claude plugin marketplace add https://github.com/wobsoriano/logbook
claude plugin install logbook@logbook
```

Or load it locally during development:

```bash
claude --plugin-dir /path/to/logbook
```

> For other ways to configure the Obsidian MCP, see [mcp-obsidian.org/install](https://mcp-obsidian.org/install/).

## Usage

### Save a checkpoint mid-session

Run at any point to snapshot progress without ending the session. Calling it multiple times in the same session overwrites the previous checkpoint rather than creating duplicates.

```
/logbook:save
```

### Archive and exit

Run at the end of a session to save a final note and terminate Claude Code.

```
/logbook:done
```

### Resume a previous session

Run at the start of a new session to load a saved note as context. Claude will summarise what was done and suggest the next step.

```
/logbook:resume auth-refactor
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

## License

MIT
