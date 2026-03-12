# pr_review_bot — Setup

PR review.

See [documentation/dobbi_setup.md](../../../documentation/dobbi_setup.md) for shared token and access requirements.

## Calling Workflow

Add this to the calling repository:

```yaml
name: Pantheon PR Review

on:
  pull_request:
    types: [opened, synchronize, reopened]

permissions:
  contents: read
  pull-requests: write
  checks: read

jobs:
  dobbi-pr-review:
    uses: your-org/agentic-pantheon/.github/workflows/dobbi-pr_review_bot.yml@main
    with:
      repo: ${{ github.repository }}
      pr_number: ${{ github.event.pull_request.number }}
    secrets:
      github_token: ${{ secrets.PANTHEON_TOKEN }}
```
