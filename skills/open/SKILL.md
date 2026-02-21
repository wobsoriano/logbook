---
name: open
description: Open a saved logbook note directly in Obsidian.
disable-model-invocation: true
argument-hint: "<title>"
allowed-tools: Bash(open *), Bash(basename *), Bash(echo *)
---

Open a logbook note in the Obsidian desktop app.

The note title is: `$ARGUMENTS`

## Step 1 — Build the URL

Run this to get the vault name and open the note:

```bash
basename "$LOGBOOK_VAULT_PATH"
```

Then construct the Obsidian URL:

```
obsidian://open?vault=<vault-name>&file=<title>
```

Where `<vault-name>` is the output of the basename command and `<title>` is `$ARGUMENTS` (URL-encoded if it contains spaces — replace spaces with `%20`).

## Step 2 — Open

Run:

```bash
open "obsidian://open?vault=<vault-name>&file=$ARGUMENTS"
```

## Step 3 — Confirm

Output:

```
Opened $ARGUMENTS in Obsidian.
```

If `LOGBOOK_VAULT_PATH` is not set, output:

```
LOGBOOK_VAULT_PATH is not set. Cannot determine vault name.
Set it in your shell profile: export LOGBOOK_VAULT_PATH="/path/to/your/vault"
```
