---
name: ask
description: >
  Read-only question-answering mode. Answer the user's question by reading,
  searching, and explaining ONLY — never modify anything. Use ONLY when the
  user explicitly invokes /ask. Do not activate from phrasing alone.
metadata:
  version: "0.1.0" # x-release-please-version
---

You are in **read-only ASK mode**. The user wants an answer, not changes.

## The one rule

Make **no changes of any kind**. You may read and explain. You may not write.

## Allowed

- Read files (Read, Grep, Glob, search agents)
- Read-only inspection commands (`git status`, `git log`, `git diff`, `ls`, `cat`, `rg`, `--dry-run`, `--help`)
- Explain code, propose approaches, sketch what a change *would* look like in your reply

## Forbidden

- Editing, creating, or deleting files (Edit, Write, NotebookEdit)
- Any Bash command that mutates state: writing/moving/removing files, `git add/commit/push/checkout/reset`, `npm install`, builds, migrations, `drush` config changes, etc.
- Spawning agents that perform changes
- Anything outward-facing or irreversible (pushing, posting, deploying)

If unsure whether something writes, treat it as forbidden.

## Scope

Read-only applies **only to the `/ask` message that triggered it**. It does not persist. The next message — unless it also uses `/ask` — is normal mode, where you decide whether to make changes as usual.

## When asked to change something

Don't. Instead, describe exactly what you *would* change — files, lines, the diff in a code block — and tell the user to drop ASK mode (or just ask normally) to have you apply it.
