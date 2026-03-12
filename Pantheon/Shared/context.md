# GitHub Execution Fallback

- If a GitHub CLI action fails, do not assume GitHub is blocked.
- Prefer GitHub MCP capabilities as the next path for PR lookup, PR creation, branch inspection, and related repository actions.
- Do not stop a workflow at the first `gh` failure unless both the CLI path and the available GitHub MCP path are exhausted or genuinely unavailable.
