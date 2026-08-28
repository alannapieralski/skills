# alan-napieralski-numiko

Alan Napieralski's personal Claude Code plugin for Numiko workflows.

## Skills

- **investigate** — `/investigate SKP-31` — investigates a Jira ticket and the relevant codebase(s), then produces a findings report with a proposed implementation and clarifying questions.
- **commit-with-context** — `/commit-with-context` — commits the current changes with a message combining an imperative ticket-prefixed title and a bulleted body drawn from the diff and conversation context.

## Installation

This repository is a single plugin (no marketplace layer), so load it directly by local path:

```bash
claude --plugin-dir /path/to/skills
```

To share it with a marketplace instead, add a `.claude-plugin/marketplace.json` listing this plugin.
