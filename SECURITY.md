# Security Policy

## Supported Versions

We release patches for security vulnerabilities for the latest major version of Agentic Pantheon.

| Version | Supported          |
| ------- | ------------------ |
| Latest | :white_check_mark: |

## Reporting a Vulnerability

If you discover a security vulnerability in this project, please report it responsibly.

**Please do not open a public GitHub issue for security vulnerabilities.**

Instead:

1. **Email the maintainer** — Send details to the repository owner/maintainer (see the GitHub profile or commit history for contact information).
2. **Include**:
   - Description of the vulnerability
   - Steps to reproduce
   - Potential impact
   - Any suggested fix (optional)

We will acknowledge receipt and aim to respond within a reasonable timeframe. We may ask for additional information to help us understand and address the issue.

## Security Best Practices for Users

When using Agentic Pantheon orchestrators:

- **Never commit secrets** — Use GitHub Actions secrets (`${{ secrets.SECRET_NAME }}`) for tokens, API keys, and credentials.
- **Principle of least privilege** — Grant orchestrators only the permissions they need (e.g., minimal scopes for `GITHUB_TOKEN`).
- **Review workflow permissions** — Check `.github/workflows/*.yml` for `permissions` blocks and restrict where possible.
- **Rotate credentials** — If you suspect a secret may have been exposed, rotate it immediately.

## Out of Scope

The following are generally out of scope for security reports:

- Issues in third-party services (GitHub, Slack, Jira, Sentry, etc.)
- Social engineering or physical access attacks
- Denial of service that requires significant resources to exploit
