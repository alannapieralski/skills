# alan-napieralski-numiko

Alan Napieralski's personal Claude Code plugin for Numiko workflows.

## Skills

- **investigate** — `/investigate SKP-31` — investigates a Jira ticket and the relevant codebase(s), then produces a findings report with a proposed implementation and clarifying questions.
- **commit-with-context** — `/commit-with-context` — commits the current changes with a message combining an imperative ticket-prefixed title and a bulleted body drawn from the diff and conversation context.
- **ask** — `/ask` — read-only question-answering mode for a single message: reads, searches, and explains without making any changes.

## Installation

This repository is also its own marketplace, so it can be installed through the Claude Code desktop app or CLI:

```bash
claude plugin marketplace add alannapieralski/skills
claude plugin install my@alan-napieralski-numiko
```

Or add it as a local path instead of a GitHub remote if you're developing against a local clone:

```bash
claude plugin marketplace add /path/to/skills
```

For a one-off local test without installing, load the plugin directly:

```bash
claude --plugin-dir /path/to/skills
```
