---
name: save
description: Checkpoint the current session to Obsidian without ending it. Use to save progress mid-session.
disable-model-invocation: true
allowed-tools: Bash(git branch --show-current), Bash(basename *), mcp__obsidian__write_note
---

Archive this Claude Code session to the Obsidian vault and continue.

## Step 1 — Gather metadata

Run these to get context:

```bash
git branch --show-current 2>/dev/null || echo "no-branch"
basename "$PWD"
```

## Step 2 — Analyze the conversation

Review the full conversation history and extract:

- **Title**: Check if `/logbook:save` was already invoked during this conversation — if so, reuse the exact same title it used (to update the existing checkpoint rather than create a duplicate). Otherwise, derive a 2–4 word title, lowercase with hyphens (e.g. `auth-refactor`, `todo-api-setup`).
- **Continued-from**: Check if `/logbook:resume` was invoked during this conversation — if so, note the title of the note that was resumed.
- **Summary**: 2–3 sentences on what has been accomplished so far.
- **Key decisions**: Choices made during the session with brief rationale.
- **What was done**: Specific implementations, changes, commands, or findings so far.
- **Open questions / follow-ups**: Anything unresolved or to revisit.
- **Context for next session**: File paths, commands, state, or gotchas that would help someone resume quickly without re-reading the whole session.

## Step 3 — Mermaid diagram (if helpful)

If the session involved architecture, a workflow, data flow, component relationships, or a multi-step process — include a `mermaid` diagram that captures it. Skip if the session was straightforward Q&A or simple edits.

## Step 4 — Write the note

Use `mcp__obsidian__write_note` to write the note at path `<title>.md` (root of vault, no subfolder). This overwrites any existing note at that path (i.e. updates the checkpoint if save was already called this session).

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

## Step 5 — Confirm

After the note is saved, output exactly:

```
Saved → <title>.md
Session:  ${CLAUDE_SESSION_ID}
Branch:   <branch>
```

Then continue the session normally.
