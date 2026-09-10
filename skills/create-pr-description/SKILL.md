---
name: create-pr-description
description: Write a pull request description aimed at a reviewer who has never seen the project or the ticket, plus inline review notes for the parts of the diff that need them. Use when the user asks for a PR description, a pull request write-up, or says something like "write this up for the PR" or "describe this branch", or invokes "/create-pr-description <branch>".
disable-model-invocation: true
metadata:
  version: "0.1.0" # x-release-please-version
---

# Create PR description

Produce a pull request description for a reviewer who has never seen this
project or the ticket, and a separate set of inline review notes for anything in
the diff that a reviewer would otherwise stumble on.

## Invocation

```
/create-pr-description <target-branch> [anything to emphasise]
```

The target branch is what the PR will merge into (`master`, `main`,
`develop`, `sprint/q8`). If it is not given, check `git branch --show-current`
and the repo's usual base, then ask rather than assume, since diffing against
the wrong branch produces a confidently wrong description.

Everything after the branch is context the user wants reflected. Treat it as
high-signal: they are telling you what they think matters, which is often
something the diff alone cannot show.

## Gathering the material

Use these sources in priority order. Each one fills gaps the one above leaves;
none of them replaces the others.

1. **What the user said in the prompt.** If they flagged specific files,
   trade-offs, or things they want called out, that is the strongest signal in
   the whole run. Some of it belongs in the description, some belongs in the
   review notes, and some is not worth including. Decide deliberately and tell
   them what you left out.

2. **The current conversation.** If the work happened in this session, you
   already know the real reasoning behind each decision, including the dead
   ends. This is more accurate than any artefact, because it captures intent
   rather than result.

3. **Commit messages.** `git log <branch>..HEAD --format='%h %s%n%b' --no-merges`
   Good commit bodies explain why a change was made and what alternative was
   rejected. In a fresh session with no conversation history, these are your
   main source.

4. **The diff.** `git diff <branch>...HEAD --stat` for shape, then
   `git diff <branch>...HEAD --name-status`, then read the files that matter.
   Note the three dots: you want the changes on the branch, not unrelated
   movement on the base.

5. **The ticket reference.** Usually in the branch name or the commit subject
   prefix. Include it in the title if you find one. Never invent ticket
   rationale you were not given.

**The final diff decides what is in.** Commit messages describe intent at the
time of writing, and some of those changes get reverted, rewritten or
superseded later in the branch. If a change is not in
`git diff <branch>...HEAD`, it does not go in the description, however
prominently a commit message features it.

## Writing the description

Read both of these every time, before writing a line:

- `references/principles.md` — what belongs in the description and why, how
  length scales with complexity, and the accuracy rules.
- `references/structure.md` — the exact shape of the output at each size.

## Before you output

Read the description once as someone who has never opened this repo. The place
where you would ask a question is the place to add a sentence.

Then check it against both reference files, counting the limits rather than
eyeballing them.

## Output

Print the description in the conversation as markdown, ready to paste into the
PR. Then print the review notes as a clearly separate section, since those are
inline comments for specific lines rather than part of the description.

Do not write files unless asked, and do not post to the PR host.

Close by flagging, in a line or two, any judgement call worth the user's
attention: something you interpreted differently from how they described it,
something you deliberately left out, or something the diff could not confirm.

## House style

British English. No em dashes. Match the tone of the repo's existing commit
messages and PR descriptions where you can see them.
