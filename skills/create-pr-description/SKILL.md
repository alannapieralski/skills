---
name: create-pr-description
description: Write a pull request description aimed at a reviewer who has never seen the project or the ticket, by reading the commit history, the diff against a target branch, the current conversation and anything the user says to emphasise. Use this whenever the user asks for a PR description, pull request description, PR write-up, or says something like "write this up for the PR", "describe this branch", "I need a description for this pull request", or "/create-pr-description <branch>". Also use it when a branch is finished and the user is about to raise a PR, even if they do not name the skill.
disable-model-invocation: true
---

# Create PR description

Produce a pull request description that makes the code easier to review, and a
separate set of inline review notes for anything in the diff that a reviewer
would otherwise stumble on.

## Who you are writing for

The reader is a competent developer who has **never seen this project, has not
read the ticket, and does not know what was asked for**. They have to decide
whether to approve the code. Every line of the description exists to remove a
question they would otherwise have to ask in a comment.

That single idea drives everything else: what to include, what to cut, and how
long the description should be. A description that reads well but leaves the
reviewer guessing has failed. So has one that is exhaustive enough to answer
everything but too long for anyone to read.

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

## Read these before writing

- `references/principles.md` — what belongs in the description and why, how
  length scales with complexity, and the accuracy rules.
- `references/structure.md` — the exact shape of the output at each size.

Read both every time. They are short, and the structure file is what keeps
successive runs consistent rather than freestyled.

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

Follow `references/structure.md` for the shape and `references/principles.md`
for the judgement calls. In outline:

1. Work out the size tier from the diff and the nature of the changes, not from
   file count alone.
2. Write the intro. Three beats maximum, always.
3. Group the changes by intent, not by file, and write one line per change.
4. Add a scope or testing note only if the reviewer needs one.
5. Decide which parts of the diff deserve inline review notes. Often none do.

## Before you output

Check each of these, because they are the failure modes that actually happen:

- **Every claim is supported by the diff.** Where the user's framing and the
  code disagree, follow the code and tell the user about the difference in
  plain terms rather than silently picking one. Getting this wrong is worse
  than saying less.
- **The intro is three beats or fewer.** Count them.
- **Nothing states how, only what and why.** No mechanisms, selectors,
  specificity, or step-by-step logic anywhere in the description. That is
  implementation detail; leave it out unless the user asked about that part.
- **Every bullet is twenty words or fewer.** Count the long ones.
- **Nothing is padded.** If a bullet only restates its group heading, or a
  section exists because the template has one, cut it.
- **A stranger could follow it.** Read it once as someone who has never opened
  this repo. The place where you would ask a question is the place to add a
  sentence.

## Output

Print the description in the conversation as markdown, ready to paste into the
PR. Then print the review notes as a clearly separate section, since those are
inline comments for specific lines rather than part of the description.

Do not write files unless asked, and do not post to the PR host. Writing
directly to Bitbucket via MCP is not currently authenticated in this setup; if
that changes, posting would slot in here as a step that always confirms the
final text with the user first.

Close by flagging, in a line or two, any judgement call worth the user's
attention: something you interpreted differently from how they described it,
something you deliberately left out, or something the diff could not confirm.

## House style

British English. No em dashes. Match the tone of the repo's existing commit
messages and PR descriptions where you can see them.
