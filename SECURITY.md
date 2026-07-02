# Security Policy

This repository contains guidance and sample configuration for using AI coding agents safely. Treat changes to this repository as security-relevant.

## Reporting issues

Open a GitHub issue or contact the repository owner for:

- unsafe agent instructions
- overbroad tool permissions
- misleading security guidance
- broken Mermaid diagrams
- stale official documentation links
- risky MCP recommendations
- examples that could cause accidental production mutation

Do not include real secrets, credentials, customer data, or exploit payloads that target third-party systems.

## Contribution expectations

Security-sensitive changes require careful review, especially changes to:

- `.github/agents/`
- `.github/prompts/`
- `.github/copilot-instructions.md`
- `.github/workflows/`
- `.vscode/`
- `docs/02-threat-model.md`
- `docs/03-secure-configuration-baseline.md`
- `SECURITY.md`

## Review standard

Contributions should:

- prefer least privilege
- avoid wildcard tool access as a default
- separate planning, implementation, and review
- treat prompt injection as a real risk
- include evidence and references for product-specific behavior
- avoid implying that any single control is complete protection
