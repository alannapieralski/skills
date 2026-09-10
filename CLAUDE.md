# Working in this repo

This repo is Alan Napieralski's personal Claude Code plugin, `alann`, served by its own
single-plugin marketplace. It contains no application code and no build tooling. Almost every
change is a `SKILL.md`, or an edit to `.claude-plugin/marketplace.json`.

## `skills/` vs `.claude/skills/`

Two directories here both hold a `SKILL.md`, kept apart by audience:

- **`skills/`** ships. Everything under it is part of the `alann` plugin, installed by anyone who
  adds this marketplace.
- **`.claude/skills/`** doesn't ship. It loads only when working in this repo itself, for
  conventions about developing the plugin rather than using it — `conventional-commit` is the
  example.

The plugin's `source` is the repo root (`"./"`), so this separation is worth stating precisely:
auto-discovery finds `skills/` at the plugin root and does **not** recurse into `.claude/`.
Verified directly, with `conventional-commit` present:

```bash
claude --plugin-dir . plugin details alann
```

reports `Skills (4)` — the four in `skills/`, not the repo-development one. Re-run it after adding
any new directory that contains a `SKILL.md`, and confirm the count is what you expect.

## Invariants

- A skill's frontmatter `name` matches its folder name and is kebab-case.
- Personal skills are invoked deliberately, not inferred: a skill sets
  `disable-model-invocation: true` unless there is a reason for Claude to reach for it unprompted.
  `ask` is currently the only skill without the flag, and relies on its description alone to hold
  the line.
- No generated files. If something needs generating to stay correct, that is a sign it should not
  exist yet.
- A new skill must be registered in `release-please-config.json` and
  `.release-please-manifest.json`, and carry a `metadata.version` in its `SKILL.md` frontmatter.
  This does not happen automatically — release-please's `packages` map is keyed by literal path and
  has no glob support.

## Commit messages

Commits must follow Conventional Commits, because release-please parses the history to decide which
skill changed and how to bump it. The `.claude/skills/conventional-commit` skill carries the format
and the type-selection rules, and auto-loads when working here — follow it.

It overrides `skills/commit-with-context`. That skill writes `TICKET-123 Title` messages for client
project work; this repo has no tickets, and a ticket-prefixed title parses as no type at all, so it
would bump nothing.

## After any change

```bash
npx -y @anthropic-ai/claude-code@latest plugin validate .
```
