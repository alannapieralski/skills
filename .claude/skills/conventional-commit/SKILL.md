---
name: conventional-commit
description: Write commit messages in Conventional Commits format (type(scope): description). Apply this whenever drafting or reviewing a commit message in this repo, so release-please can detect and version each skill correctly. Takes precedence over any ticket-prefixed commit convention, including the commit-with-context skill this repo itself ships.
---

# Conventional Commit

This repo uses [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) so that
release-please can parse commit history to bump each skill's version automatically (see
`CLAUDE.md`). Every commit message you draft here must follow this format, unless the user
explicitly asks otherwise.

## This overrides the ticket-prefixed format

This repo ships `skills/commit-with-context`, which writes `TICKET-123 Title` commit messages. That
format is for client project work. It does not apply here: this repo has no tickets, and a
ticket-prefixed title parses as no type at all, so release-please would bump nothing.

If asked to commit here via `/commit-with-context`, use that skill's process — inspect the diff,
gather rationale from the conversation, stage files by name — but write the message in the format
below instead of its ticket-prefixed one, and say that you have done so.

## Before drafting the message

1. Run `git status` and `git diff --cached` (or `git diff` if nothing is staged yet) to see exactly
   what changed.
2. Read the diff, don't guess from filenames. The message must describe what actually changed.
3. Stage files by name. Never `git add -A` or `git add .`.

## Message structure

```
type(scope): description

body (optional)

footer (optional)
```

- **type** - one of `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`,
  `chore`, `revert`.
- **scope** - optional, in parentheses. Name the skill folder affected (e.g.
  `feat(investigate): ...`). Omit it when the change is repo-wide.
- **description** - required. Imperative mood ("add", not "added"), no trailing period, under about
  72 characters.
- **body** - optional. Explain what changed and why when the summary line isn't enough on its own.
- **footer** - optional. Use `BREAKING CHANGE: <details>` for a breaking change, or reference an
  issue (`Closes #123`).

A breaking change can also be marked with `!` right after the type/scope instead of a footer:
`feat!: description` or `feat(scope)!: description`.

## Markdown isn't automatically "docs" in this repo

`type` depends on whether the file is *about* the repo or *is* something that ships:

- **Shipped skill content** - `skills/**` (a `SKILL.md`, its `references/*.md`) - these files are
  the product: editing them changes what a consuming agent actually does. Type by effect, same as
  you would for code: `fix` for a correction (removing a vague or wrong rule), `feat` for a new
  rule or capability, `refactor` for reorganizing without changing behaviour. `docs` doesn't apply
  here even though the files are markdown.
- **Repo meta-documentation** - `README.md`, `CLAUDE.md`, and this `.claude/skills/` directory -
  these describe or support the repo itself, not something installed. `docs` is correct for these.

## Hidden types silently skip a release

Before a release PR is even considered, release-please generates changelog notes from the pending
commits and skips the release entirely if that text comes out empty. `docs`, `chore`, `test`,
`build` and `ci` are hidden in `release-please-config.json`, so a batch of only those produces no
release. Typing a real skill change as `docs` drops its version bump silently.

## Examples

```
feat(investigate): add a codebase-scope question to the report
fix(ask): drop the ambiguous read-only wording
refactor(create-pr-description): split structure guidance into references
docs: clarify how to register a new skill with release-please
chore: rename the plugin from 'my' to 'alann'
ci: validate the marketplace manifest on pull requests
feat(ask)!: require explicit invocation

BREAKING CHANGE: /ask no longer activates from phrasing alone
```

## Validation

- `type` must be one of the allowed values above.
- `description` is required and must use the imperative mood.
- `scope` is optional but improves clarity, given this repo tracks per-skill versions with
  release-please's manifest mode.
- Don't invent a type that isn't in the list, even if it reads more naturally - the fixed set is
  what release-please and changelog tooling parse against.
