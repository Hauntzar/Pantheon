# pr_developer_bot — Setup

PR comment resolution.

See [documentation/dobbi_setup.md](../../../documentation/dobbi_setup.md) for shared token and access requirements.

## Extra Secrets

- `SLACK_BOT_TOKEN`

## Trigger

Trigger this bot from:

- `pull_request_review_comment`
- `issue_comment` on PRs

## Calling Workflow

Add this to the calling repository:

```yaml
name: Pantheon PR Developer

on:
  pull_request_review_comment:
    types: [created, edited]
  issue_comment:
    types: [created, edited]

permissions:
  contents: write
  pull-requests: write

jobs:
  dobbi-pr-fix:
    if: ${{ github.event.pull_request || github.event.issue.pull_request }}
    uses: your-org/agentic-pantheon/.github/workflows/dobbi-pr_developer_bot.yml@main
    with:
      repo: ${{ github.repository }}
      pr_number: ${{ github.event.pull_request.number || github.event.issue.number }}
      comment_id: ${{ github.event.comment.id }}
    secrets:
      github_token: ${{ secrets.PANTHEON_TOKEN }}
      slack_token: ${{ secrets.SLACK_BOT_TOKEN }}
```
