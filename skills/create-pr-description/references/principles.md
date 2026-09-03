# Principles

What belongs in a PR description, what does not, and how much of it to write.

## The test every line has to pass

A reviewer who has never seen this project is going to read the diff. Some
things they can work out from the code, and some things they cannot. **The
description covers what the code cannot tell them.**

The code tells them: what the change does, how it is implemented, which files
moved. Do not repeat any of that.

The code cannot tell them: why this was needed, what problem it solves, why it
was done this way rather than the obvious way, what the boundaries of the change
are, and whether the parts that look alarming are deliberate. That is the whole
job of the description.

If a bullet would make a reader think "yes, I can see that in the diff", cut it.

## Length scales with complexity, not with effort

This is the rule most often broken, so be deliberate about it. A PR that took
three days but changes one thing in a familiar way gets a short description. A
PR that took an afternoon but introduces a pattern nobody in the repo has used
before needs more.

**Complexity means:** the change is cross-cutting, or it touches many files, or
it departs from patterns already established in the codebase, or it does
something for the first time, or it contains a workaround whose reasoning is
not visible in the code.

**Complexity does not mean:** the diff is large because of generated files,
reformatting, or the same small change repeated in twenty places. That is a big
diff and a simple PR. Say so, and keep it short.

Never pad a small PR to look substantial. A three-bullet description on a small
change is a good description, and a reviewer will thank you for it.

## What, not how

Every sentence in the description, intro beats and bullets alike, says what
changed and, where it is not obvious, why. It never says how: no mechanisms,
no selectors, no specificity weights, no step-by-step logic, no naming of the
particular property or function that did it. That belongs in the code itself
or in a commit message. Only bring it into an inline review note, and only
when a reviewer genuinely could not work out the *reasoning* without it, never
just to narrate the mechanism.

Keep sentences short and plain. A beat or a bullet is one sentence, not a
compound one stitched together with colons, semicolons, or a chain of clauses
explaining the reasoning inline. Bullets are twenty words or fewer; if a
bullet needs more to make sense, it has picked up an implementation detail,
so cut it or move it to a commit message.

Do not explain implementation details unless the user has explicitly asked
about that specific part of the change.

## What earns a place

**The reason the work happened.** One clause is usually enough. "The filter was
returning stale results after a facet change" tells the reviewer more than three
sentences about what the fix does.

**Anything done for the first time.** A new mixin, a new breakpoint, a new
directory convention, the first use of a library. Reviewers apply more scrutiny
to precedent than to instances of it, and they need to know they are looking at
precedent.

**Work that was necessary but is not what the ticket asked for.** Refactors,
clean-up, and fixes discovered along the way. Left unexplained, these read as
scope creep, and the reviewer will ask about them. Say why they were needed,
briefly.

**The boundary of the change.** When the diff touches shared or global code, say
what the blast radius actually is and how you established that. This is often
the single most reassuring line in the whole description: it turns "why is this
PR touching thirty files?" into "fine, I know where to look."

**Deliberate decisions that look wrong.** Hardcoded values, a duplicated block
that could have been abstracted, an exception to a rule. If you predict the
comment, pre-empt it.

## What to leave out

- A file-by-file walkthrough. Group by intent instead.
- Step-by-step explanations of how the code works. The code is in the diff.
- Implementation detail of any kind: mechanics, mechanisms, specificity ties,
  exact selector weights, which line won on source order, which property or
  function did it. The commit history keeps this; the description should not
  repeat it, unless the user has explicitly asked about that part.
- Headings with a single bullet under them.
- A testing section that only says the work was tested locally. Either say what
  was actually covered, or leave it out.
- Restating the ticket title in longer words.

## Accuracy

Be careful here, because a confidently wrong PR description costs more review
time than no description at all.

- Verify claims against the diff before making them. If the user says a theme
  was deleted and the diff shows a config map was removed while the class
  remains, describe what the diff shows and tell the user why you differed.
- Do not describe intent you were not given. Where you cannot tell why something
  changed, describe what it does and say the reasoning is not captured, rather
  than inventing a plausible motive.
- Where a claim is about behaviour that cannot be seen in the diff, such as
  visual regression testing or manual QA, attribute it to the author rather than
  asserting it as fact.

## Inline review notes

The second deliverable, and often the more useful one. These are comments the
author pastes onto specific lines in the PR, pre-empting the questions a
reviewer would ask there.

Write one only where the code is genuinely hard to review:

- It introduces something new to the codebase.
- It is a workaround, and the obvious approach does not work. Explain why not,
  since that is the part the reviewer cannot reconstruct.
- It looks like a mistake but is deliberate.
- It removes or renames something, and the evidence that this is safe lives
  outside the diff.
- It is knowingly incomplete, with a follow-up ticket to come.

Explain the reasoning, not the mechanism. The diff already shows what the code
does and how; the note only needs to carry what the diff cannot: why. If a
draft note is walking through selectors, specificity, or the steps the code
takes, cut that part and keep only the why.

Keep each to one to three short sentences, anchor it to a file and a line
where useful, and write it in the author's voice so it can be pasted without
editing.

If nothing in the diff qualifies, say so. Most PRs need no review notes at all,
and manufacturing them wastes the reviewer's attention on code that was fine.

## Tone

Plain and direct. Present tense for what the code now does, past tense for what
was done to it. No enthusiasm, no selling, no apologising for the size of the
diff. Assume the reader is competent and short of time.
