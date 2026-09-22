---
name: commit-with-context
description: Commit the current uncommitted and staged changes, split into one or more commits by concern, each with a short imperative title and a high-level bulleted body covering the idea and the rationale from the conversation, not a line-by-line changelog. Invoke with /commit-with-context {optional ticket context}
disable-model-invocation: true
metadata:
  version: "0.1.2" # x-release-please-version
---

# Commit With Context

## Usage

```
/commit-with-context
/commit-with-context {ticket ID or context, if not already discussed}
```

No argument is needed if ticket context was already established earlier in the conversation. Only pass one if the ticket has not come up yet and should be included.

## Instructions

### 1. Inspect the change set

Run:
- `git status`, every modified, added, and untracked file
- `git diff`, unstaged changes
- `git diff --cached`, already-staged changes
- `git log --oneline -10`, recent style, for tone only. The structural format below always applies regardless of the repository's existing convention.

### 2. Find the ticket ID

The ticket ID is mandatory. It is not optional context, it must prefix the commit title (see format below). Look for it in this order:

1. The current branch name, run `git branch --show-current`. In this convention the branch is named after the ticket (for example `TEEN-650`), so the branch name usually is the ticket ID.
2. The conversation, or the argument passed to this skill, if a ticket ID was already mentioned there.
3. If neither yields a ticket ID, stop and ask the user for it before doing anything else. Do not guess, invent, or continue without one.

Beyond the ID itself, also check the conversation for any ticket title or description. That is optional context for a bullet, unlike the ID it can be omitted if it does not exist.

### 3. Group the changes into commits

Read the full diff and split it into one or more commits along concerns, not along files. A concern is a single coherent idea (a fix, a feature, a refactor); unrelated concerns that happen to sit in the same working tree get separate commits.

Two pulls in tension here, resolve them in this order:

1. **Never commit a broken intermediate state.** If change A only makes sense, compiles, or passes with change B (a function and its only caller, a config key and the code that reads it, a rename split across files), they go in the *same* commit even if they touch different concerns on paper. Splitting them apart would mean an earlier commit leaves the tree broken or mid-thought.
2. **Otherwise, split by concern.** If two changes are each independently coherent and neither depends on the other to make sense, put them in separate commits rather than bundling them into one "misc changes" commit.

Decide the grouping before staging anything. For each group, note which files (or, if a single file mixes two concerns, which hunks) belong to it.

### 4. Gather rationale

Note any reasons, trade-offs, or decisions already discussed for why the change was made this way, not just what changed. Gather this per group where the discussion differed by concern.

### 5. Stage and commit each group

For each group, in an order where earlier commits don't depend on later ones: stage its files by name (never `git add -A` or `git add .`; use `git add -p` when a single file needs splitting across two commits). Review the staged diff for anything that looks like a secret before committing. Write the message using the format below, then verify it (step 6) before committing via heredoc. Repeat until every group is committed. This skill only creates local commits, never push unless separately asked.

### 6. Verify the line lengths

Before each commit, check the drafted message against the limits in the format section: title line ≤ 50 characters, every body line (each bullet) ≤ 72 characters. Count characters directly rather than estimating. If a line is over, shorten it, don't just note the excess. Only commit once every line passes.

## Commit message format

Exactly two parts: a title line, then a bulleted body. Never a free-form paragraph in the body.

```
<TICKET-ID> <Title, imperative, capitalised, no trailing period, 50 characters max overall>

- <bullet>
- <bullet>
- <bullet>
```

### Title rules

- Always prefix the title with the ticket ID found in step 2, followed by a space. This is not optional and never omitted.
- After the prefix, summarise the net effect of the whole diff in one line, not a list of files touched.
- Use the imperative mood ("Add", "Fix", "Refactor", not "Added" or "Fixes").
- 50 characters is the hard limit for the whole line, prefix included. Shorter is better. Trim the summary, never the ticket ID, to fit.

### Bullet rules

Stay high level. The body explains the idea and the reason for it, not a changelog of every line touched: not what happened hunk by hunk, but why the diff exists.

- Ticket bullet: an optional extra bullet with the ticket's title or description, include it only when that extra context already exists (from the conversation or the invocation argument), beyond the ID itself which already lives in the title.
- Summary bullet: one bullet giving the high-level shape of the change, the problem it addresses and the approach taken, fused together rather than split into a separate "what" bullet per file or function touched. Name a file only when the message would otherwise be too vague to locate the change.
- Rationale bullet: only when there is a non-obvious trade-off or rejected alternative worth recording beyond the summary bullet's "why".
- Most commits need one or two bullets total, not one per file or per hunk. If a commit's body would need many bullets to cover everything touched, that is usually a sign the commit should have been split further in step 3.
- One line per bullet. Omit a category entirely when it has nothing to say (for example, no ticket bullet when there is no extra ticket context beyond the ID).
- 72 characters is the hard limit per body line, including the leading `- `. Wrap a long bullet onto a continuation line indented two spaces rather than exceeding it (see the rationale bullet in the example below).

## Source of each part

- Grouping: derived from reading the whole diff in step 3, not from file names alone.
- Ticket ID prefix: derived from the current branch name, or the conversation, or the invocation argument, in that order. Never invented, always confirmed with the user first if genuinely unavailable.
- Title summary and summary bullet: derived from the group's own diff.
- Ticket bullet: derived only from context already surfaced in the conversation or passed as an argument to this skill. Never invented or looked up independently.
- Rationale bullet: derived from the conversation's discussion of why the change was made.

## Example

On branch `SEARCH-142`, a diff that both debounces the search input and, unrelated, fixes a typo in the results empty-state copy. Two concerns, two commits:

```
SEARCH-142 Debounce the search input API calls

- Search was firing an API call on every keystroke; debounce the
  input handler so it fires once typing settles.
- Chose a fixed delay over cancel-on-keystroke, since the existing
  fetch already de-duplicates in-flight requests.
```

```
SEARCH-142 Fix typo in empty-state message

- "No reuslts found" read wrong when a search returns nothing.
```
