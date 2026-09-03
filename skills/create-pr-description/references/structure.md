# Structure

The shape of the output. Pick a tier from the nature of the change, then follow
that tier's template. The tiers exist so that successive runs produce
recognisably similar documents rather than a new invention each time.

## Choosing a tier

| Tier | When | Description length |
|---|---|---|
| A. Small | One clear intent, familiar patterns, few files | Intro plus a flat list |
| B. Standard | Several related changes, or one change with side work | Intro plus grouped bullets |
| C. Complex | Cross-cutting, many files, new patterns, or clean-up alongside the feature | Intro, grouped bullets, scope note, review notes |

Judge on the nature of the change, not the line count. Twenty files touched by
the same trivial rename is tier A. Four files introducing a pattern the repo has
never used is tier B or C.

When you are between two tiers, choose the smaller one. It is easier for a
reviewer to ask a follow-up question than to read past padding.

---

## Tier A: small

```markdown
# <TICKET-REF> <Short title, sentence case>

<One or two beats: what this does and why it was needed.>

- <Change, one line.>
- <Change, one line.>
- <Change, one line.>
```

No section headings. No groups. Three to six bullets. If you find yourself
wanting a heading, you are probably in tier B.

---

## Tier B: standard

```markdown
# <TICKET-REF> <Short title, sentence case>

<Two or three beats: what this delivers, and why, and any scope boundary worth
stating up front.>

## What has changed

**<Group name>**

- <Change, one line.>
- <Change, one line.>

**<Group name>**

- <Change, one line.>
```

Two to four groups. A group with one bullet should be merged into another group
or promoted into the intro.

---

## Tier C: complex

```markdown
# <TICKET-REF> <Short title, sentence case>

<Beat one: what this PR delivers, in plain terms.>

<Beat two: why the extra work was necessary, if it is not obvious.>

<Beat three: the scope boundary, if a reviewer would otherwise be alarmed by
the size or spread of the diff.>

## What has changed

**<Group name>**

- <Change, one line.>
- <Change, one line.>

**<Group name>**

- <Change, one line.>

## Scope and testing

<One short paragraph or a few bullets: what was verified, and how the blast
radius of any shared changes was established.>
```

Then, printed separately in the chat and not part of the description:

```markdown
# Inline comments (paste onto the relevant lines)

**`<path/to/file>`**
> <One to three sentences, in the author's voice, explaining what a reviewer
> could not work out from the code alone.>
```

---

## Section rules

**Title.** Ticket reference, then a short sentence-case summary. Where the PR
does two things, such as a feature and the clean-up it required, name both, as
in `EC-1618 Learning resources: redesign step 1 and theming clean-up`. Bitbucket
takes the title separately, so this line doubles as the suggested PR title.

**Intro. Three beats maximum, always, at every tier.** This is a hard limit,
because it is the part everyone reads and the part that most easily sprawls. A
beat is one short, plain sentence, not a compound one stitched together with
colons, semicolons, or a chain of clauses working through the reasoning.
Three beats, three short sentences, and no more:

1. What this PR delivers, at the highest level: what was asked for and what
   was done. Plain terms, no jargon, no implementation detail.
2. Why it was done this way, if that is not already obvious from beat one.
   Skip this if the first beat already answers it.
3. The scope boundary, when the diff would otherwise look wider than the ticket.
   Skip it when the change is contained and obviously so.

Beats two and three are optional. Beat one is not. If a beat is running long,
it has picked up an implementation detail, so cut it. If you are reaching for
a fourth beat, the material belongs in a bullet, not the intro.

**Groups.** Named by intent, not by file or directory. Good group names describe
outcomes: `Redesign`, `Typography`, `Theming clean-up`, `Component fixes`. Bad
ones describe locations: `SCSS changes`, `Config`, `Templates`.

**Bullets.** One line each, twenty words or fewer. State what changed at the
level a reviewer skimming would grasp, not the mechanism that achieves it. Add
the reason only where it is not obvious from the group heading. If a bullet is
running long or explaining a mechanism, that detail belongs in the code, a
commit message, or a review note, not here.

**Scope and testing.** Tier C, and tier B when the PR touches shared code.
Answer the question the reviewer is actually asking, which is "what else might
this have broken, and how do you know it did not?" Attribute manual testing to
the author rather than stating it as established fact.

**Bold headings for groups, `##` for sections.** Keep the heading levels shallow.
A PR description with `###` in it is too long.

---

## Worked example: tier C intro

For a branch that redesigned a section of a site and cleaned up its theming
along the way:

> This PR delivers the first step of the learning resources redesign: a new
> hero, the Education Brand palette, a new content font, and updated layout
> for the summary box and location bar.
>
> It also cleans up a lot of inconsistent theming underneath the redesign,
> since several components could not be fixed by changing a colour alone.
>
> Everything is scoped to the learning section. A few shared files are
> touched, but nothing changes visually elsewhere.

Three short beats. The first says what was done, at a glance. The second says
why a redesign ticket also contains a refactor, which is the reviewer's first
question. The third defuses the spread of changed files, which is their
second. None of the three explains how; the mechanics stay out of the intro
entirely.
