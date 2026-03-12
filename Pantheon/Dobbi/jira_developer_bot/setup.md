# jira_developer_bot — Setup

Jira story implementation.

See [documentation/dobbi_setup.md](../../../documentation/dobbi_setup.md) for shared token and access requirements.

## Extra Secrets

- `JIRA_API_TOKEN`
- `JIRA_EMAIL`
- `JIRA_SITE_URL`
- `SLACK_BOT_TOKEN`

## Trigger

Trigger this bot with `repository_dispatch` using event type `jira-story-assigned`.

Required `client_payload` fields:

- `ticket_key`
- `channel` optional

## Calling Workflow

Add this to the calling repository:

```yaml
name: Pantheon Jira Developer

on:
  repository_dispatch:
    types: [jira-story-assigned]

permissions:
  contents: write
  pull-requests: write

jobs:
  dobbi-jira-dev:
    uses: your-org/agentic-pantheon/.github/workflows/dobbi-jira_developer_bot.yml@main
    with:
      ticket_key: ${{ github.event.client_payload.ticket_key }}
      channel: ${{ github.event.client_payload.channel }}
    secrets:
      github_token: ${{ secrets.PANTHEON_TOKEN }}
      jira_token: ${{ secrets.JIRA_API_TOKEN }}
      jira_email: ${{ secrets.JIRA_EMAIL }}
      jira_site_url: ${{ secrets.JIRA_SITE_URL }}
      slack_token: ${{ secrets.SLACK_BOT_TOKEN }}
```
