---
name: load
description: Load a previously saved session note from Obsidian as context. Use at the start of a session to pick up where you left off.
disable-model-invocation: true
argument-hint: "<title>"
allowed-tools: mcp__obsidian__read_note, mcp__obsidian__search_notes
---

Load a previously saved session note from the Obsidian vault into the current session's context.

The title to load is: `$ARGUMENTS`

## Step 1 — Read the note

Attempt to read the note at path `$ARGUMENTS.md` using `mcp__obsidian__read_note`.

If the note is not found, use `mcp__obsidian__search_notes` to find notes with titles or content similar to `$ARGUMENTS`, list the top matches, and ask the user which one to load.

## Step 2 — Internalize the context

Once the note is loaded, read it carefully. Pay particular attention to:

- **Context for Next Session** — the most actionable section for resuming
- **Open Questions / Follow-ups** — what was left unresolved
- **What Was Done** — what has already been completed (don't redo it)
- **Key Decisions** — constraints and rationale already established

## Step 3 — Confirm and summarise

Output a brief confirmation so the user knows what was loaded, then summarise what the previous session covered in 2–4 bullet points based on the note. End with what the natural next step would be based on the open questions or context.

Format:

```
Loaded ← <title>.md
Project: <project> · Branch: <branch> · <date>

<2–4 bullet summary of previous session>

Ready to continue. Suggested next step: <next step from context>
```
