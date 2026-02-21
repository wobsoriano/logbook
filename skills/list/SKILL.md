---
name: list
description: List recently saved logbook notes from the Obsidian vault.
disable-model-invocation: true
allowed-tools: mcp__obsidian__list_directory, mcp__obsidian__get_notes_info
---

List saved logbook notes from the Obsidian vault.

## Step 1 — Get all notes

Use `mcp__obsidian__list_directory` to list all files at the vault root (path `/`).

## Step 2 — Get metadata

From the file list, filter to `.md` files only. Use `mcp__obsidian__get_notes_info` to fetch metadata for up to the 15 most recently modified ones.

## Step 3 — Display

Read the frontmatter of each note and display a table sorted by date descending. Only include notes that have a `session_id` or `project` frontmatter field (i.e. notes saved by logbook, not unrelated vault notes).

Format:

```
 Title                  Project          Branch      Date
 ─────────────────────────────────────────────────────────
 auth-refactor          my-app           main        2026-02-21
 todo-api-setup         my-app           feat/api    2026-02-20
 logbook-plugin         claude-vault     main        2026-02-19
```

If no notes are found, output:

```
No logbook notes found in vault.
```
