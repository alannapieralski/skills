# Commit message format

Exactly two parts: a title line, then a bulleted body. Never a free-form paragraph in the body.

```
<TICKET-ID> <Title, imperative, capitalised, no trailing period, 50 characters max overall>

- <bullet>
- <bullet>
- <bullet>
```

## Title rules

- Always prefix the title with the ticket ID found in step 2, followed by a space. This is not optional and never omitted.
- After the prefix, summarise the net effect of the whole diff in one line, not a list of files touched.
- Use the imperative mood ("Add", "Fix", "Refactor", not "Added" or "Fixes").
- 50 characters is the hard limit for the whole line, prefix included. Shorter is better. Trim the summary, never the ticket ID, to fit.

## Bullet rules

Stay high level. The body explains the idea and the reason for it, not a changelog of every line touched: not what happened hunk by hunk, but why the diff exists.

- Ticket bullet: an optional extra bullet with the ticket's title or description, include it only when that extra context already exists (from the conversation or the invocation argument), beyond the ID itself which already lives in the title.
- Summary bullet: one bullet giving the high-level shape of the change, the problem it addresses and the approach taken, fused together rather than split into a separate "what" bullet per file or function touched. Name a file only when the message would otherwise be too vague to locate the change.
- Rationale bullet: only when there is a non-obvious trade-off or rejected alternative worth recording beyond the summary bullet's "why".
- Most commits need one or two bullets total, not one per file or per hunk. If a commit's body would need many bullets to cover everything touched, that is usually a sign the commit should have been split further in step 3.
- One line per bullet. Omit a category entirely when it has nothing to say (for example, no ticket bullet when there is no extra ticket context beyond the ID).
- 72 characters is the hard limit per body line, including the leading `- `. Wrap a long bullet onto a continuation line indented two spaces rather than exceeding it (see the rationale bullet in the example below).

## Attribution

Never add a `Co-Authored-By` trailer, or any other attribution line, to the commit message.

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
