---
name: search
description: Search saved logbook notes by keyword or topic. Use when you remember something from a past session but not the exact title.
disable-model-invocation: true
argument-hint: "<query>"
allowed-tools: mcp__obsidian__search_notes, mcp__obsidian__get_frontmatter
---

Search saved logbook notes in the Obsidian vault.

The search query is: `$ARGUMENTS`

## Step 1 — Search

Use `mcp__obsidian__search_notes` with the query `$ARGUMENTS`. Request up to 10 results.

## Step 2 — Filter

Filter results to notes that have logbook frontmatter (`session_id` or `project` field) to avoid surfacing unrelated vault notes.

## Step 3 — Display

For each matching note, fetch its frontmatter with `mcp__obsidian__get_frontmatter` and display:

```
 Title                  Project          Branch      Date
 ─────────────────────────────────────────────────────────
 auth-refactor          my-app           main        2026-02-21
 todo-api-setup         my-app           feat/api    2026-02-20
```

Below the table, add a tip:

```
Use /logbook:resume <title> to load a note as context.
```

If no results match, output:

```
No notes found for "$ARGUMENTS".
```
