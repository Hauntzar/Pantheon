# Runtime Rules

- Store and/or load new useful memories in their associated memory location
- Keep memories concise and precise, with one line explanations.
- Finish the workflow unless it is blocked or needs escalation.
- Before prompting the user, ask yourself if the workflow is finished, and attempt to finish it if it is not.
- Assume the MCP runners have all MCPs authorized, look up documentation for each MCP integration if needed.
- If it is blocked because of permissions, try again with the associated MCP
- Do not use Seer.
- Avoid manual browser work when possible.
- Assign issues and stories to the user if a PR is created for it.
- For changes, always checkout the repo to environments/ and perform the necessary changes there.

## Where Memories Live

- Shared conventions: `Pantheon/Shared/context.md`
- Project maps: `Pantheon/Shared/projects/{project}.md`
- Repo memory: `Pantheon/{cluster}/memories/{repo_name}/context.md`
- Repo preferences: `Pantheon/{cluster}/memories/{repo_name}/preferences.md`
- Jira history: `Pantheon/{cluster}/memories/{repo_name}/jira/history.md`
- Epic memory: `Pantheon/{cluster}/memories/{repo_name}/jira/{EPIC_KEY}/context.md`
- Private bot memory: `Pantheon/{cluster}/{orchestrator}/memories/`

## How To Resolve Context

Start from the workflow inputs such as `repo`, `ticket_key`, `channel`, `pr_number`, or `comment_id`.

- If you have a repo, find the project map whose repositories include it.
- If you have a Jira key, find the project map whose Jira project matches the key prefix.
- Use that project map to discover related repos, channels, teams, and naming conventions.
- Derive the repo short name from the repo slug and load that repo's memory.
