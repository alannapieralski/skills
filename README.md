# alan-napieralski-numiko

Alan Napieralski's personal Claude Code plugin for Numiko workflows, installed as `alann`.

These are personal preferences, not Numiko house standards — those live in
[numiko/numiko-frontend-marketplace](https://github.com/numiko/numiko-frontend-marketplace).

## Skills

Each is invoked explicitly; none activate from phrasing alone.

<details>
<summary><strong>investigate</strong> — <code>/investigate SKP-31</code></summary>

Investigates a Jira ticket and the relevant codebase(s), then produces a findings report with a proposed implementation and clarifying questions.

Use when:
- You have a Jira ticket ID or URL and want analysis before implementation begins
- You want the ticket read fully before any codebase digging starts, so findings are interpreted against it rather than the other way round

</details>

<details>
<summary><strong>commit-with-context</strong> — <code>/commit-with-context</code></summary>

Commits the current uncommitted and staged changes with a message combining an imperative ticket-prefixed title and a bulleted body drawn from the diff and conversation context.

Use when:
- You want a commit message that captures the ticket, the diff, and the reasoning discussed in conversation, not just a one-line summary

</details>

<details>
<summary><strong>create-pr-description</strong> — <code>/create-pr-description &lt;branch&gt;</code></summary>

Writes a pull request description for a reviewer who has seen neither the project nor the ticket, from the commit history, the diff against a target branch, and the conversation.

Use when:
- You're opening a PR and want a description plus inline review notes for anything in the diff a reviewer would otherwise stumble on

</details>

<details>
<summary><strong>ask</strong> — <code>/ask</code></summary>

Read-only question-answering mode for a single message: reads, searches, and explains without making any changes.

Use when:
- You want an answer, not a change — a single-turn question that must not touch any files

</details>

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
