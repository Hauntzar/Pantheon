# bug_bot — Setup

Bug triage and resolution from Slack + Sentry.

See [documentation/dobbi_setup.md](../../../documentation/dobbi_setup.md) for shared token and access requirements.

## Extra Secrets

- `SLACK_BOT_TOKEN`
- `SENTRY_AUTH_TOKEN`
- `JIRA_API_TOKEN`
- `JIRA_EMAIL`
- `JIRA_SITE_URL`

## Trigger

Trigger this bot with `repository_dispatch` using event type `sentry-alert`.

Required `client_payload` fields:

- `message`
- `channel`
- `thread_ts` optional

## Calling Workflow

Add this to the calling repository:

```yaml
name: Pantheon Bug Triage

on:
  repository_dispatch:
    types: [sentry-alert]

permissions:
  contents: write
  pull-requests: write

jobs:
  bug-triage:
    uses: your-org/agentic-pantheon/.github/workflows/dobbi-bug_bot.yml@main
    with:
      slack_message: ${{ github.event.client_payload.message }}
      channel: ${{ github.event.client_payload.channel }}
      thread_ts: ${{ github.event.client_payload.thread_ts }}
    secrets:
      github_token: ${{ secrets.PANTHEON_TOKEN }}
      slack_token: ${{ secrets.SLACK_BOT_TOKEN }}
      sentry_token: ${{ secrets.SENTRY_AUTH_TOKEN }}
      jira_token: ${{ secrets.JIRA_API_TOKEN }}
      jira_email: ${{ secrets.JIRA_EMAIL }}
      jira_site_url: ${{ secrets.JIRA_SITE_URL }}
```
