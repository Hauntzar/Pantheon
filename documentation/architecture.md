# Architecture — Agentic Pantheon

A centralised library of AI **orchestrators** invoked from GitHub Actions
workflows in any repository. Each orchestrator is an autonomous agent that
coordinates tasks across MCP (Model Context Protocol) tool servers.

## Objective

Introduce a reusable AI orchestration platform to automate engineering
workflows and foster cross-team knowledge sharing around agentic AI.

## System Overview

```mermaid
flowchart TB
    subgraph triggers["External Triggers"]
        PR["Pull Request Event"]
        SLACK["Slack Message"]
        JIRA["Jira Webhook"]
        MANUAL["Manual Dispatch"]
    end

    subgraph calling_repo["Calling Repository"]
        CW["Calling Workflow<br/><code>.github/workflows/*.yml</code>"]
    end

    subgraph pantheon["agentic-pantheon"]
        RW["Reusable Workflow<br/><code>.github/workflows/{cluster}-{bot}.yml</code>"]

        subgraph runtime["Agent Runtime"]
            CODEX["OpenAI Codex Agent"]
            MCP["MCP Servers<br/>GitHub · Filesystem · Slack<br/>Jira · Sentry · Shell"]
        end

        subgraph instructions["Instruction Hierarchy"]
            BASE["Pantheon/base.md<br/>Memory system · Runtime resolution"]
            CBASE["Pantheon/{cluster}/base.md<br/>Identity · Escalation · Security"]
            AGENT["Pantheon/{cluster}/{bot}/agent.md<br/>Workflow steps · Risk rules"]
        end

        subgraph memory["Memory System"]
            SHARED["Shared/projects/{project}.md<br/>Repo maps · Channels · Conventions"]
            REPO_MEM["memories/{repo}/context.md<br/>Architecture · Style · Tests"]
            PREFS["memories/{repo}/preferences.md<br/>Exemptions · Overrides · Tone"]
            JIRA_MEM["memories/{repo}/jira/<br/>History · Epic context"]
            BOT_MEM["{bot}/memories/<br/>Private learnings · State"]
        end
    end

    subgraph outputs["Outputs"]
        GH_OUT["GitHub<br/>PR reviews · Branches · PRs"]
        SLACK_OUT["Slack<br/>Status updates · Escalations"]
        JIRA_OUT["Jira<br/>Tickets · Comments · Transitions"]
    end

    triggers --> CW
    CW -->|"workflow_call"| RW
    RW -->|"checkout repos<br/>install MCP servers"| CODEX
    CODEX <--> MCP
    CODEX -->|"loads"| BASE --> CBASE --> AGENT
    CODEX -->|"reads/writes"| memory
    CODEX --> outputs
```

---

## Instruction Inheritance

```mermaid
flowchart LR
    A["Pantheon/base.md<br/><i>Memory system<br/>Runtime context resolution</i>"] --> B["Pantheon/{cluster}/base.md<br/><i>Identity · Behaviour<br/>Escalation · Security</i>"]
    B --> C["Pantheon/{cluster}/{bot}/agent.md<br/><i>Workflow steps<br/>Risk thresholds · I/O</i>"]

    style A fill:#e8f4fd,stroke:#1a73e8
    style B fill:#fce8e6,stroke:#d93025
    style C fill:#e6f4ea,stroke:#1e8e3e
```

Every agent loads **all three layers** before acting. Instructions are
additive — each layer adds specificity without repeating what the parent
already defines.

---

## Memory Layers

```mermaid
flowchart TD
    L1["<b>Layer 1 — Shared</b><br/>Pantheon/Shared/projects/{project}.md<br/><i>Repo ↔ Jira maps · Slack channels · Teams</i>"]
    L2["<b>Layer 2 — Cluster</b><br/>Pantheon/{cluster}/memories/{repo}/<br/><i>context.md · preferences.md</i>"]
    L3["<b>Layer 3 — Jira</b><br/>memories/{repo}/jira/<br/><i>history.md · {EPIC_KEY}/context.md</i>"]
    L4["<b>Layer 4 — Bot-Private</b><br/>{bot}/memories/<br/><i>Learnings · State · Tuning</i>"]

    L1 -->|"readable by<br/>ALL agents"| L2
    L2 -->|"shared across<br/>cluster agents"| L3
    L3 -->|"model-populated<br/>from Atlassian MCP"| L4

    style L1 fill:#e8f4fd,stroke:#1a73e8
    style L2 fill:#fce8e6,stroke:#d93025
    style L3 fill:#fff3e0,stroke:#e65100
    style L4 fill:#f3e8fd,stroke:#7b1fa2
```

| Layer | Scope | Written by | Example |
|-------|-------|-----------|---------|
| **Shared** | All agents | Human | Project → repo map, Slack channels |
| **Cluster** | All bots in a cluster | Human + model | Repo architecture, coding style, preferences |
| **Jira** | All bots in a cluster | Model (from Jira via MCP) | Epic details, activity history |
| **Bot-Private** | Single bot only | Model | Learnings, retry state, heuristics |

---

## Runtime Context Resolution

Agents never hardcode repos, channels, or users. Everything is resolved dynamically:

```mermaid
flowchart LR
    A["Workflow inputs<br/><i>repo · ticket_key · channel<br/>pr_number · comment_id</i>"]
    B["Project map<br/><i>Pantheon/Shared/projects/*.md</i>"]
    C["Cluster memories<br/><i>context.md · preferences.md</i>"]
    D["Agent executes<br/>with full context"]

    A -->|"identify target"| B -->|"resolve repos<br/>channels · conventions"| C -->|"load architecture<br/>style · thresholds"| D
```

---

## Execution Flow (GitHub Actions)

```mermaid
sequenceDiagram
    participant Caller as Calling Repo
    participant Workflow as Reusable Workflow
    participant Codex as OpenAI Codex
    participant MCP as MCP Servers
    participant Memory as Pantheon Memory

    Caller->>Workflow: workflow_call (inputs + secrets)
    Workflow->>Workflow: Checkout calling repo + agentic-pantheon
    Workflow->>Workflow: Install MCP server packages
    Workflow->>Codex: prompt (agent.md path + inputs)

    Codex->>Memory: Load base.md → cluster base.md → agent.md
    Codex->>Memory: Resolve project map → load repo memories
    Codex->>MCP: Use tools (GitHub, Slack, Jira, Sentry, etc.)

    loop Workflow Steps
        Codex->>MCP: Execute step (fetch data, analyse, implement)
        MCP-->>Codex: Results
    end

    opt Medium-risk work
        Codex->>Codex: Karen verification pass
    end

    Codex->>Memory: Update memories (history, learnings)
    Codex-->>Workflow: Structured outputs (status, URLs, counts)
    Workflow-->>Caller: Job outputs
```

---

## Directory Layout

```
agentic-pantheon/
├── Pantheon/
│   ├── base.md                              # Global: memory system + runtime resolution
│   ├── Shared/
│   │   └── projects/{project_name}.md       # Project → repo/channel maps
│   └── {cluster}/
│       ├── base.md                          # Cluster: identity, escalation, security
│       ├── memories/{repo_name}/
│       │   ├── context.md                   # Repo architecture & conventions
│       │   ├── preferences.md               # Behaviour overrides
│       │   └── jira/
│       │       ├── history.md               # Running activity log
│       │       └── {EPIC_KEY}/context.md    # Epic details
│       └── {bot}/
│           ├── agent.md                     # Full workflow instructions
│           ├── setup.md                     # Secrets, integration, examples
│           └── memories/                    # Bot-private state
│
├── .github/
│   ├── agents/{cluster}-{bot}.md            # Copilot Chat agent definitions
│   └── workflows/{cluster}-{bot}.yml        # Reusable GitHub Actions workflows
│
├── templates/                               # Scaffolding for new orchestrators
└── documentation/
    ├── architecture.md                      # ← this file
    └── enable_pantheon.md                   # Setup & onboarding guide
```

---

## Key Principles

| Principle | Description |
|-----------|-------------|
| **Dynamic resolution** | Repos, channels, users, and conventions are never hardcoded in agent instructions — always resolved from workflow inputs + project maps |
| **Layered inheritance** | `base.md` → `cluster/base.md` → `agent.md` — each layer adds specificity |
| **Risk-gated action** | Low → auto-act, Medium → verify with Karen, High → escalate to human |
| **Shared memory** | Cluster-level memories are shared across all bots; bot-private memories are isolated |
| **Portable orchestrators** | Any repo can call any orchestrator by adding a workflow + secrets — no changes to agentic-pantheon required |

---
