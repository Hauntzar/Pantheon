# Contributing to Agentic Pantheon

Thank you for your interest in contributing to Agentic Pantheon. This document provides guidelines for contributing.

## How to Contribute

### Reporting Bugs

- Open an issue with a clear, descriptive title
- Include steps to reproduce, expected vs actual behavior, and your environment
- Check existing issues to avoid duplicates

### Suggesting Features

- Open an issue describing the feature and use case
- Discuss the design before submitting a large change

### Pull Requests

1. **Fork the repository** and create a branch from `main`
2. **Make your changes** — keep them focused and well-scoped
3. **Follow existing conventions** — match the style of agent instructions, workflows, and documentation
4. **Update documentation** if you add orchestrators, change behavior, or modify setup
5. **Submit a PR** with a clear description of what changed and why

### Adding a New Orchestrator

1. Use the structure under `Pantheon/{cluster}/{orchestrator}/`:
   - `agent.md` — full agent instructions and workflow
   - `setup.md` — secrets, invocation guide, and examples
2. Add a reusable workflow in `.github/workflows/` that invokes the orchestrator
3. Add the orchestrator to the README table and document required secrets
4. See [documentation/architecture.md](documentation/architecture.md) for design patterns

## Code and Documentation Style

- **Markdown**: Use clear headings, code blocks with language tags, and tables where appropriate
- **YAML**: Use 2-space indentation; keep workflows readable and well-commented
- **Agent instructions**: Be explicit and structured; reference `base.md` and cluster-level context

## Secrets and Security

- **Never commit secrets** — API keys, tokens, passwords, or credentials
- Use `${{ secrets.SECRET_NAME }}` in workflow examples; document required secrets in `setup.md`
- See [SECURITY.md](SECURITY.md) for reporting security issues

## License

By contributing, you agree that your contributions will be licensed under the Apache License 2.0.
