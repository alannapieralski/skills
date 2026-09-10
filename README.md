# alan-napieralski-numiko

Alan Napieralski's personal Claude Code plugin for Numiko workflows, installed as `alann`.

These are personal preferences, not Numiko house standards — those live in
[numiko/numiko-frontend-marketplace](https://github.com/numiko/numiko-frontend-marketplace).

## Skills

Each is invoked explicitly; none activate from phrasing alone.

- **investigate** — `/investigate SKP-31` — investigates a Jira ticket and the relevant codebase(s), then produces a findings report with a proposed implementation and clarifying questions.
- **commit-with-context** — `/commit-with-context` — commits the current changes with a message combining an imperative ticket-prefixed title and a bulleted body drawn from the diff and conversation context.
- **create-pr-description** — `/create-pr-description <branch>` — writes a pull request description for a reviewer who has seen neither the project nor the ticket, from the commit history, the diff against a target branch, and the conversation.
- **ask** — `/ask` — read-only question-answering mode for a single message: reads, searches, and explains without making any changes.

## Install

This repository is also its own marketplace, so it can be installed through the Claude Code desktop app or CLI:

```bash
claude plugin marketplace add alannapieralski/skills
```

```bash
claude plugin install alann@alan-napieralski-numiko
```

Or add it as a local path instead of a GitHub remote if you're developing against a local clone:

```bash
claude plugin marketplace add /path/to/skills
```

For a one-off local test without installing, load the plugin directly:

```bash
claude --plugin-dir /path/to/skills
```

## Update

Two separate things go stale, and each has its own command.

```bash
claude plugin marketplace update alan-napieralski-numiko
```

```bash
claude plugin update alann@alan-napieralski-numiko
```

The first refreshes the catalogue — what plugins exist and where each one points. The second
re-fetches the installed plugin's content against that entry. Either needs a restart (or
`/reload-plugins`) to take effect.

## Versioning

Each skill carries its own semver in its `SKILL.md` frontmatter, bumped automatically by
[release-please](https://github.com/googleapis/release-please) from Conventional Commits touching
that skill's folder. Each skill's `CHANGELOG.md` lives in its own directory.

Adding a skill means registering it by hand in `release-please-config.json` and
`.release-please-manifest.json` — see [CLAUDE.md](CLAUDE.md).
