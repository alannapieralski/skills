---
name: commit-with-context
description: Commit the current uncommitted and staged changes, split into one or more commits by concern, each with a short imperative title and a high-level bulleted body covering the idea and the rationale from the conversation, not a line-by-line changelog. Invoke with /commit-with-context {optional ticket context}, or pass noid to skip the ticket ID entirely.
disable-model-invocation: true
argument-hint: "[ticket ID or context | noid]"
metadata:
  version: "0.1.3" # x-release-please-version
---

# Commit With Context

## Usage

```
/commit-with-context
/commit-with-context {ticket ID or context, if not already discussed}
/commit-with-context noid
```

No argument is needed if ticket context was already established earlier in the conversation. Only pass one if the ticket has not come up yet and should be included.

Pass `noid`, or say elsewhere in the conversation that the ticket ID should be left out, to opt out of the ticket ID entirely — see step 2.

## Instructions

### 1. Inspect the change set

Run:
- `git status`, every modified, added, and untracked file
- `git diff`, unstaged changes
- `git diff --cached`, already-staged changes
- `git log --oneline -10`, recent style, for tone only. The structural format below always applies regardless of the repository's existing convention.

### 2. Find the ticket ID

The ticket ID is mandatory and must prefix the commit title (see `references/format.md`), unless the user opts out.

**Opt-out:** if the invocation argument is `noid`, or the user has otherwise said not to include a ticket ID, skip this step entirely: no lookup, no ask, and the title carries no ticket prefix (see `references/format.md`).

Otherwise, look for it in this order:

1. The current branch name, run `git branch --show-current`. In this convention the branch is named after the ticket (for example `TEEN-650`), so the branch name usually is the ticket ID.
2. The conversation, or the argument passed to this skill, if a ticket ID was already mentioned there.
3. If neither yields a ticket ID, stop and ask the user for it before doing anything else. Do not guess, invent, or continue without one — unless they choose to opt out at that point instead.

Beyond the ID itself, also check the conversation for any ticket title or description. That is optional context for a bullet, unlike the ID it can be omitted if it does not exist.

### 3. Group the changes into commits

Read the full diff and split it into one or more commits along concerns, not along files. A concern is a single coherent idea (a fix, a feature, a refactor); unrelated concerns that happen to sit in the same working tree get separate commits.

Two pulls in tension here, resolve them in this order:

1. **Never commit a broken intermediate state.** If change A only makes sense, compiles, or passes with change B (a function and its only caller, a config key and the code that reads it, a rename split across files), they go in the *same* commit even if they touch different concerns on paper. Splitting them apart would mean an earlier commit leaves the tree broken or mid-thought.
2. **Otherwise, split by concern.** If two changes are each independently coherent and neither depends on the other to make sense, put them in separate commits rather than bundling them into one "misc changes" commit. Sitting in the same file, the same section, or being small is not a reason to merge them, that's a matter of concern, not location or size.

Decide the grouping before staging anything. For each group, note which files (or, if a single file mixes two concerns, which hunks) belong to it.

### 4. Gather rationale

Note any reasons, trade-offs, or decisions already discussed for why the change was made this way, not just what changed. Gather this per group where the discussion differed by concern.

### 5. Stage and commit each group

For each group, in an order where earlier commits don't depend on later ones: stage its files by name (never `git add -A` or `git add .`; use `git add -p` when a single file needs splitting across two commits). Review the staged diff for anything that looks like a secret before committing. Write the message using `references/format.md`, then verify it (step 6) before committing via heredoc. Repeat until every group is committed. This skill only creates local commits, never push unless separately asked.

### 6. Verify the line lengths

Before each commit, check the drafted message against the limits in `references/format.md`: title line ≤ 50 characters, every body line (each bullet) ≤ 72 characters. Count characters directly rather than estimating. If a line is over, shorten it, don't just note the excess. Only commit once every line passes.

## Commit message format

Read `references/format.md` before drafting each message. It has the exact
two-part structure, the title and bullet rules, the no-attribution rule, where
each part is sourced from, and a worked example.
