---
name: done
description: Archive the current session to Obsidian and end. Use when wrapping up a session.
disable-model-invocation: true
argument-hint: "[cost]"
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

Review the full conversation and extract:

- **Title**: 2–4 words, lowercase with hyphens (e.g. `auth-refactor`, `todo-api-setup`). Shortest title that still makes the note easy to find later.
- **Summary**: 2–3 sentences on what was accomplished.
- **Key decisions**: Choices made during the session with brief rationale.
- **What was done**: Specific implementations, changes, commands, or findings.
- **Open questions / follow-ups**: Anything unresolved or to revisit.
- **Context for next session**: File paths, commands, state, or gotchas that would help someone resume quickly without re-reading the whole session.

## Step 3 — Mermaid diagram (if helpful)

If the session involved architecture, a workflow, data flow, component relationships, or a multi-step process — include a `mermaid` diagram that captures it. Skip if the session was straightforward Q&A or simple edits.

## Step 4 — Write the note

Use `mcp__obsidian__write_note` to create the note at path `<title>.md` (root of vault, no subfolder).

The cost is: `$ARGUMENTS` — if this is non-empty, use it as the `cost` field value. If empty, set it to `null`.

Note format:

```markdown
---
session_id: ${CLAUDE_SESSION_ID}
branch: <branch or "no-branch">
project: <basename of working directory>
date: <today's date, YYYY-MM-DD>
cost: <$ARGUMENTS if provided, otherwise null>
---

# <Title>

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

## Step 5 — Confirm, then terminate

After the note is saved, output exactly:

```
Archived → <title>.md
Session:  ${CLAUDE_SESSION_ID}
Branch:   <branch>
```

Then immediately run this bash command to terminate the session:

```bash
kill -INT $PPID
```
