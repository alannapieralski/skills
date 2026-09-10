---
name: commit-with-context
description: Commit the current uncommitted and staged changes with a message combining a short imperative title and a bulleted body drawn from the diff, any ticket context already discussed in the conversation, and the conversation's rationale. Invoke with /commit-with-context {optional ticket context}
disable-model-invocation: true
metadata:
  version: "0.1.1" # x-release-please-version
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

### 3. Gather rationale

Note any reasons, trade-offs, or decisions already discussed for why the change was made this way, not just what changed.

### 4. Stage and commit

Stage the relevant files by name (never `git add -A` or `git add .`). Review the staged diff for anything that looks like a secret before committing. Write the message using the format below, then verify it (step 5) before committing via heredoc. This skill only creates a local commit, never push unless separately asked.

### 5. Verify the line lengths

Before committing, check the drafted message against the limits in the format section: title line ≤ 50 characters, every body line (each bullet) ≤ 72 characters. Count characters directly rather than estimating. If a line is over, shorten it, don't just note the excess. Only commit once every line passes.

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

- Ticket bullet: an optional extra bullet with the ticket's title or description, include it only when that extra context already exists (from the conversation or the invocation argument), beyond the ID itself which already lives in the title.
- Change bullets: describe what was actually changed, grounded in the diff, file by file or behaviour by behaviour, whichever reads more clearly.
- Rationale bullets: capture the why from the conversation, the problem being solved, the approach chosen, and any alternative ruled out.
- One line per bullet. Omit a category entirely when it has nothing to say (for example, no ticket bullet when there is no extra ticket context beyond the ID).
- 72 characters is the hard limit per body line, including the leading `- `. Wrap a long bullet onto a continuation line indented two spaces rather than exceeding it (see the rationale bullet in the example below).

## Source of each part

- Ticket ID prefix: derived from the current branch name, or the conversation, or the invocation argument, in that order. Never invented, always confirmed with the user first if genuinely unavailable.
- Title summary and change bullets: derived from `git diff` and `git diff --cached`.
- Ticket bullet: derived only from context already surfaced in the conversation or passed as an argument to this skill. Never invented or looked up independently.
- Rationale bullets: derived from the conversation's discussion of why the change was made.

## Example

On branch `SEARCH-142`, given a diff adding debounce logic to a search input, and a conversation that already established the ticket is about search firing an API call on every keystroke:

```
SEARCH-142 Debounce the search input API calls

- Search input was firing an API call on every keystroke.
- Added a 300ms debounce to the input handler in search-box.js before dispatching the fetch.
- Chose a fixed 300ms delay over cancel-on-keystroke, since the existing fetch
  already de-duplicates in-flight requests.
```
