---
name: investigate
description: Investigate a Jira ticket and relevant codebases, then produce a concise findings report with a proposed implementation and focused clarifying questions. Use when given a Jira ticket ID or URL (e.g. SKP-31, PROJ-123) and the user wants analysis before implementation begins. No plan protocol, no approval gate, just investigation + report + questions.
disable-model-invocation: true
metadata:
  version: "0.1.1" # x-release-please-version
---

# Investigate

## Usage

```
/investigate SKP-31
/investigate SKP-31 <any additional context, constraints, or steering notes>
```

## Instructions

> **Ticket first, always.** The ticket is the source of truth. Read and fully understand it — summary, description, acceptance criteria, linked issues, comments — *before* touching the codebase. Do not start any codebase investigation until you can state, in your own words, what the ticket actually requires. Everything you find in the code is interpreted *against* the ticket, not the other way around.

### 1 — Read the ticket

If no ticket ID was provided, check the current git branch name — it follows the convention `PROJ-123` (e.g. `OFGEM-42`, `KU-7`). Extract the ticket ID from the branch name and use that. If no ticket ID can be found in either place, stop and ask the user before continuing.

Fetch the full Jira ticket: summary, description, acceptance criteria, linked issues, comments. Understand it before doing anything else. Do not proceed to any of the steps below until the ticket is fully understood.

### 1b — Detect existing work on this branch

Before the general codebase sweep, determine whether this branch *already has work on it* — typically backend (BE) work that a previous session or teammate landed.

Detect it: get the current branch name and the ticket ID, then scan recent commits (`git log --oneline -30`) for any whose message references the branch/ticket ID (e.g. `BT-532 ...`). If one or more such commits exist, treat this as a **branch with existing work** and run the analysis below. If none do, skip to step 2.

When existing work is found:

- **Read the actual diff**, not just commit subjects. Use `git log -p` / `git diff main...HEAD` (substitute the real main branch) to see exactly what changed — new/modified entities, fields, paragraph/content types, config, services, controllers, hooks, schema, migrations, API/endpoints, render arrays, preprocess functions, template stubs.
- **Sanity-check the BE against the ticket.** Does what was built actually satisfy the ticket's description and acceptance criteria? Call out explicitly: what the BE covers, anything in the ticket it does *not* yet cover, and anything the BE does that the ticket did *not* ask for (scope creep or misread). If the BE diverges from the ticket, say so plainly — that's a flag, not a footnote.
- **Derive the immediate FE follow-up from the BE.** The whole point: by reading what the backend now exposes, identify the *specific* frontend work that should happen next. Be concrete — name the fields/data the BE made available and the exact templates, SDCs, Twig files, JS, and styles that now need to consume them. Point to file paths where the FE work will live. This becomes a directly actionable "do this next" list, not a vague gesture at "the frontend".

### 2 — Investigate in parallel

Run all of the following simultaneously:

- **Codebase** — find existing components, templates, SDCs, config, or patterns directly relevant to the ticket
- **Reference implementations** — if the ticket or context mentions other projects or repos, look for them in `~/Developer/` on the local machine. If a referenced repo is not found there, stop and ask the user to provide it before continuing.
- **Design** — if a Figma URL is in the ticket or context, fetch the design context for relevant nodes

### 3 — Report

Produce a concise report in this structure:

**What the ticket requires**
One short paragraph.

**Findings**
Bullet points: what exists, what relevant patterns were found across codebases, notable similarities/differences between reference implementations.

**Existing work on this branch** *(only if step 1b found any)*
- What the BE already does, summarised from the actual diff.
- Whether it matches the ticket — what's covered, what's missing, anything built beyond scope.
- **FE follow-up (do next):** the specific frontend work the BE now enables — named fields/data, exact template/SDC/Twig/JS/style files to touch, with file paths.

**Key implementation details**
Specific code-level observations: file paths, component names, config structure, patterns worth copying vs adapting. Be concrete.

**Proposed implementation**
A clear, actionable proposal. Mention what to copy verbatim, what to adapt, what to build from scratch. Flag anything project-specific that should be stripped for a framework context.

### 4 — Ask clarifying questions

After the report, ask only the questions that are genuinely uncertain — things that, if assumed incorrectly, would mean implementing the wrong thing. Keep it focused: 3–6 questions max, no padding.

Do not begin implementing anything until the user responds.
