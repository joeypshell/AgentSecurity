# Copilot / Agent Pilot Intake

Use this template before enabling agent mode, cloud/coding agent, MCP, or write-capable tools for a team or repository.

## Request summary

| Field | Answer |
| --- | --- |
| Request title |  |
| Requestor |  |
| Business owner |  |
| Security owner |  |
| Engineering team |  |
| Requested start date |  |
| Requested end/review date |  |
| Capability class | Inline / Chat / Edit / VS Code Agent / Visual Studio Agent / Cloud Agent / Read-only MCP / Write MCP / Local MCP / Third-party MCP |
| Broad rollout or pilot |  |

## Business purpose

Describe the workflow, expected benefit, and why lower-risk modes such as Ask or Plan are insufficient.

## Scope

| Field | Answer |
| --- | --- |
| Users/groups |  |
| Repositories |  |
| Data classification | Public / Internal / Confidential / Regulated / Secrets-adjacent / Production |
| IDE/client | VS Code / Visual Studio / GitHub.com / CLI / Other |
| Devices | Managed only? Y/N |
| Network/location restrictions |  |

## Requested capabilities

Check all that apply.

- [ ] Standard Copilot Chat / Ask
- [ ] Edit mode
- [ ] VS Code agent mode
- [ ] Visual Studio agent mode
- [ ] Copilot cloud/coding agent
- [ ] Terminal command execution
- [ ] Browser/fetch/web tools
- [ ] GitHub MCP Server read-only
- [ ] GitHub MCP Server write-capable tools
- [ ] Internal MCP server
- [ ] Third-party MCP server
- [ ] Local stdio MCP server
- [ ] Preview/beta model or feature

## Required controls

| Control | Status | Evidence link |
| --- | --- | --- |
| Enterprise-managed Copilot account |  |  |
| Approved GitHub org/enterprise identity |  |  |
| Repo sensitivity tier reviewed |  |  |
| Branch protection/rulesets |  |  |
| Required PR review |  |  |
| CODEOWNERS on sensitive paths |  |  |
| Required CI/status checks |  |  |
| Secret scanning |  |  |
| Dependency review/SCA |  |  |
| SAST or code scanning |  |  |
| Agent mode restricted to named users |  |  |
| Global auto-approval disabled |  |  |
| Terminal auto-approval disabled or narrow |  |  |
| Network egress filtered/logged |  |  |
| Sandboxing/devcontainer/VM used where appropriate |  |  |
| MCP disabled or allowlisted |  |  |
| Logs available to security |  |  |
| Incident response owner identified |  |  |

## MCP section, if applicable

| Field | Answer |
| --- | --- |
| MCP server name |  |
| Local or remote |  |
| Owner |  |
| Tool list attached |  |
| Read-only or write-capable |  |
| Auth model | OAuth / GitHub App / Fine-grained PAT / Classic PAT / Service account / Other |
| Token scope reviewed |  |
| Logs available |  |
| Registry/allowlist entry |  |
| Expiration/review date |  |

Attach `mcp-server-admission.md` for every requested MCP server.

## Red-team validation

| Scenario | Required before pilot? | Result |
| --- | --- | --- |
| Malicious README prompt injection | Yes |  |
| Terminal command escalation | Yes for agent mode |  |
| Sensitive-file edit attempt | Yes |  |
| MCP typosquat | Yes for MCP |  |
| Cross-tool exfiltration | Yes for multiple tools/MCP |  |
| Audit reconstruction drill | Yes |  |
| Visual Studio parity test | Yes for Visual Studio |  |

## Risk acceptance

| Risk | Mitigation | Residual risk owner |
| --- | --- | --- |
|  |  |  |

## Decision

- [ ] Approved baseline
- [ ] Approved pilot
- [ ] Approved exception
- [ ] Blocked
- [ ] More evidence required

Approvers:

- Security Architecture:
- Platform Engineering:
- GitHub Admin:
- Endpoint/Network, if applicable:
- Legal/Privacy, if applicable:
- Business Owner:
