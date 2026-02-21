---
name: done
description: Archive the current session to Obsidian and end. Use when wrapping up a session.
disable-model-invocation: true
allowed-tools: Bash(git branch --show-current), Bash(basename *), Bash(kill *), mcp__obsidian__write_note
---

Archive this Claude Code session to the Obsidian vault, then terminate the session.

## Step 1 — Gather metadata

Run these to get context:

```bash
git branch --show-current 2>/dev/null || echo "no-branch"
basename "$PWD"
```

## Step 2 — Analyze the conversation

Review the full conversation history and extract:

- **Title**: Check if `/logbook:save` was already invoked during this conversation — if so, reuse the exact same title it used (to overwrite the checkpoint rather than create a duplicate). Otherwise, derive a 2–4 word title, lowercase with hyphens (e.g. `auth-refactor`, `todo-api-setup`).
- **Continued-from**: Check if `/logbook:resume` was invoked during this conversation — if so, note the title of the note that was resumed.
- **Summary**: 2–3 sentences on what was accomplished.
- **Key decisions**: Choices made during the session with brief rationale.
- **What was done**: Specific implementations, changes, commands, or findings.
- **Open questions / follow-ups**: Anything unresolved or to revisit.
- **Context for next session**: File paths, commands, state, or gotchas that would help someone resume quickly without re-reading the whole session.

## Step 3 — Mermaid diagram (if helpful)

If the session involved architecture, a workflow, data flow, component relationships, or a multi-step process — include a `mermaid` diagram that captures it. Skip if the session was straightforward Q&A or simple edits.

## Step 4 — Write the note

Use `mcp__obsidian__write_note` to write the note at path `<title>.md` (root of vault, no subfolder). This overwrites any existing note at that path (e.g. from a previous `/logbook:save` checkpoint).

Note format:

```markdown
---
session_id: ${CLAUDE_SESSION_ID}
branch: <branch or "no-branch">
project: <basename of working directory>
date: <today's date, YYYY-MM-DD>
continued-from: <title of resumed note>  # omit this line if no /logbook:resume was used
---

# <Title>

> Continued from [[<continued-from-title>]]
> *(omit this line if no /logbook:resume was used)*

## Summary
<2–3 sentences>

## Key Decisions
- <decision + brief rationale>

## What Was Done
<detailed notes — include file paths, function names, commands where relevant>

## Open Questions / Follow-ups
- [ ] <item>

## Context for Next Session
<anything needed to resume quickly>

## Diagram
<mermaid block, or omit this section entirely if no diagram>
```

## Step 5 — Append to daily note

Use `mcp__obsidian__write_note` with `mode: "append"` to append a one-line entry to today's daily note at path `YYYY-MM-DD.md`:

```
- [[<title>]] · <project> · <branch>
```

Where `YYYY-MM-DD` is today's date. This keeps a running log of sessions in your daily note.

## Step 6 — Confirm, then terminate

After both writes succeed, output exactly:

```
Archived → <title>.md
Session:  ${CLAUDE_SESSION_ID}
Branch:   <branch>
```

Then immediately run this bash command to terminate the session:

```bash
kill -INT $PPID
```
