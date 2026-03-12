# Agentic Pantheon

Bobby's Agentic Pantheon — a centralised library of AI **orchestrators** that
can be called from GitHub Actions workflows in any repository.

Each orchestrator is an AI agent that coordinates a sequence of tasks across
multiple toolings and MCP (Model Context Protocol) provisions to accomplish a
defined goal.

---

## Quick Start

Add this workflow to any repository to enable the `Dobbi/pr_review_bot` orchestrator:

```yaml
# .github/workflows/pantheon-pr-review.yml
name: Pantheon PR Review

on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  pr-review:
    uses: your-org/agentic-pantheon/.github/workflows/dobbi-pr_review_bot.yml@main
    with:
      repo: ${{ github.repository }}
      pr_number: ${{ github.event.pull_request.number }}
    secrets:
      github_token: ${{ secrets.GITHUB_TOKEN }}
```

See [`documentation/github_action_example.md`](documentation/github_action_example.md)
for full examples and
[`documentation/enable_pantheon.md`](documentation/enable_pantheon.md)
for setup instructions.

---

## Repository Structure

```
agentic-pantheon/
│
├── Pantheon/
│   ├── base.md                # Base instructions ALL agents incorporate
│   │
│   ├── Shared/                # Global context — readable by all agents
│   │   └── projects/
│   │       └── {project}.md   # Maps Jira project → repos
│   │
│   └── {cluster}/               # e.g. Dobbi/
│       ├── base.md            # cluster-level base instructions
│       ├── memories/          # Shared by all bots in the cluster
│       │   └── {repo_name}/
│       │       ├── context.md
│       │       ├── preferences.md
│       │       └── jira/
│       │           ├── history.md
│       │           └── {EPIC_KEY}/
│       │               └── context.md
│       └── {orchestrator}/    # e.g. pr_review_bot/
│           ├── agent.md       # Full agent instructions & workflow
│           ├── setup.md       # Secrets & invocation guide
│           └── memories/      # Individual bot memories (private)
│
├── .github/
│   ├── agents/                # GitHub Copilot agent definitions
│   └── workflows/             # Reusable GitHub Actions workflows
│
├── templates/                 # Copy-ready scaffolding
│   ├── orchestrator/
│   ├── memories/
│   └── Shared/
│
└── documentation/
    ├── architecture.md
    ├── enable_pantheon.md
    └── github_action_example.md
```

---

## Available Orchestrators

| Cluster | Orchestrator | Description | Setup |
|---------|-------------|-------------|-------|
| Dobbi | [pr_review_bot](Pantheon/Dobbi/pr_review_bot/agent.md) | Multi-pass PR review — bugs, methodology, test coverage, description quality | [setup](Pantheon/Dobbi/pr_review_bot/setup.md) |
| Dobbi | [bug_bot](Pantheon/Dobbi/bug_bot/agent.md) | Bug triage & resolution from Sentry alerts via Slack | [setup](Pantheon/Dobbi/bug_bot/setup.md) |
| Dobbi | [jira_developer_bot](Pantheon/Dobbi/jira_developer_bot/agent.md) | Auto-implements low-risk Jira stories end-to-end | [setup](Pantheon/Dobbi/jira_developer_bot/setup.md) |
| Dobbi | [pr_developer_bot](Pantheon/Dobbi/pr_developer_bot/agent.md) | Resolves PR review comments — auto-fix or escalation | [setup](Pantheon/Dobbi/pr_developer_bot/setup.md) |

---

## Required Secrets

Each calling repository must configure the secrets its bot needs.
See the individual `setup.md` for full details.

| Secret | pr_review_bot | bug_bot | jira_developer_bot | pr_developer_bot |
|--------|:---:|:---:|:---:|:---:|
| `GITHUB_TOKEN` | **x** | **x** | **x** | **x** |
| `SLACK_BOT_TOKEN` | | **x** | **x** | **x** |
| `SENTRY_AUTH_TOKEN` | | **x** | | |
| `JIRA_API_TOKEN` | | **x** | **x** | |
| `JIRA_EMAIL` | | **x** | **x** | |
| `JIRA_SITE_URL` | | **x** | **x** | |

---

## Documentation

- [Architecture](documentation/architecture.md) — Concepts, data flow, and design decisions
- [Enable Pantheon](documentation/enable_pantheon.md) — How to use or add an orchestrator
- [GitHub Action Example](documentation/github_action_example.md) — Copy-paste workflow examples
