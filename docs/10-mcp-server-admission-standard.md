# MCP Server Admission Standard

Last reviewed: 2026-07-08

This standard defines how an enterprise should evaluate, approve, monitor, and retire MCP servers used with GitHub Copilot, VS Code, Visual Studio, cloud/coding agents, or other AI agent hosts.

MCP should be governed as **code execution plus API integration plus supply chain**. A server can expose sensitive reads, write to external systems, return prompt-injection content, execute local code, and hold credentials. Treat server approval as an engineering security decision, not a developer preference.

## 1. Admission outcomes

| Outcome | Meaning | Typical examples |
| --- | --- | --- |
| Approved baseline | Allowed for the named user group and repo class under standard controls. | Read-only internal docs MCP with approved auth and logs. |
| Approved pilot | Allowed for named users/repos and expires automatically. | GitHub MCP Server read-only with narrow toolsets. |
| Approved exception | Allowed only for a named business case with enhanced controls. | Write-capable GitHub issue/PR tooling for a platform team. |
| Blocked | Not allowed on corporate devices or with corporate data. | Public local stdio server installed through an unreviewed package. |
| Retired | Previously approved but removed from allowed list. | Unmaintained server, owner left, stale version, weak logging. |

## 2. MCP risk classes

| Class | Description | Default decision |
| --- | --- | --- |
| MCP-R1 | Remote read-only server, enterprise-owned, narrow data scope, OAuth/GitHub App, good logs. | Eligible for pilot/baseline. |
| MCP-R2 | Remote read-only third-party server with clear business purpose and documented data handling. | Pilot only after legal/privacy/security review. |
| MCP-W1 | Remote write-capable enterprise-owned server with narrow tools and rollback. | Exception only. |
| MCP-W2 | Remote write-capable third-party server. | Block by default; exception only for strong business case. |
| MCP-L1 | Local stdio server from internal signed/pinned package, running in isolated environment. | Exception only. |
| MCP-L2 | Local stdio server from public npm/pip/uvx/Docker/binary package. | Block by default. |
| MCP-U | Server with unknown owner, unknown tools, weak provenance, or unclear logging. | Block. |

## 3. Admission checklist

Every MCP server request must answer these questions before enablement.

| Field | Required answer |
| --- | --- |
| Server name | Human-readable name and package/image/URL. |
| Owner | Named team and accountable person. |
| Business purpose | Specific use case and expected benefit. |
| Requesting group | Users, teams, repositories, and data classes. |
| Deployment type | Local stdio, remote HTTP/SSE/streamable HTTP, GitHub-hosted, internal-hosted, third-party-hosted. |
| Source/provenance | Source repository, publisher, package namespace, container registry, build pipeline, signing status. |
| Version pin | Exact version, digest, or release reference. Floating latest is not acceptable for approval. |
| Tool inventory | Full list of tools with names, descriptions, inputs, outputs, and side effects. |
| Tool classification | Read, write, execute, admin, external-send, unknown. Unknown tools are treated as high risk. |
| Data classification | Public, internal, confidential, regulated, production, secrets-adjacent. |
| Allowed sources | Repositories, systems, databases, APIs, or documents the server may read. |
| Allowed sinks | Systems or APIs the server may write to or send data to. |
| Authentication | OAuth, GitHub App, service account, fine-grained PAT, classic PAT, local credential, none. |
| Authorization | How scopes, repository access, tenant access, and per-user permissions are enforced. |
| Token storage | Where credentials live and how they are rotated. Plaintext repo config is not acceptable. |
| Network destinations | Domains, ports, and internal/external services. |
| Logging | Where tool calls, target resources, user identity, errors, and write actions are logged. |
| Audit reconstruction | Whether security can reconstruct who called which tool against which target and what changed. |
| Prompt-injection exposure | Whether tool output can include user-generated, web, issue, PR, log, or other untrusted content. |
| Abuse cases | Exfiltration, unsafe writes, privilege escalation, spam, CI abuse, secret leakage, persistence. |
| Detection | SIEM/EDR/proxy/MCP logs and alert conditions. |
| Rollback | How writes are reverted and how credentials are revoked. |
| Expiration | Review date and automatic expiry for pilot/exception approvals. |

## 4. Minimum controls by server type

### Remote read-only server

- Approved registry/allowlist entry.
- OAuth or GitHub App preferred.
- Narrow scopes.
- Per-tool logging.
- Data classification review.
- No cross-tool write path without explicit approval.
- Clear owner and review date.

### Remote write-capable server

Everything required for read-only, plus:

- Business owner sign-off.
- Security architecture sign-off.
- Named users and repositories.
- Tool-specific allowlist.
- Rollback plan.
- Alerting for write/admin tools.
- Change evidence in the target system.
- Short approval duration.

### Local stdio server

Everything required for the relevant read/write class, plus:

- Strong reason it cannot be remote.
- Managed device only.
- No local admin where possible.
- Application control or signed internal package.
- Version pin or digest.
- Execution in devcontainer, VM, sandbox, or other isolation when possible.
- No access to production credentials.
- No arbitrary outbound network by default.
- EDR process and network telemetry.
- Explicit review of local file and environment-variable access.

### Third-party server

Everything required for its read/write/local/remote type, plus:

- Legal/privacy data-handling review.
- Vendor security review.
- Data retention and deletion answers.
- Breach notification and support path.
- No regulated or crown-jewel data until proven safe.
- Exit plan if the vendor changes ownership, pricing, permissions, or data terms.

## 5. GitHub MCP Server approval profiles

Start with the official GitHub MCP Server only after the MCP governance process exists.

| Profile | Scope | Allowed use | Blocked use |
| --- | --- | --- | --- |
| GitHub-MCP-RO-Minimal | Read-only URL/mode, narrow toolsets | Search, inspect repos/issues/PRs for low/medium sensitivity repos | Writes, merges, issue edits, workflow changes, secrets, admin tools |
| GitHub-MCP-RO-Security | Read-only plus security/code-search toolsets where needed | AppSec review and repo discovery | Any mutation, broad org-wide access without approval |
| GitHub-MCP-Write-Exception | Named write tools only | Specific platform workflow with rollback and logging | General developer default, broad write token, wildcard toolsets |

Required GitHub MCP controls:

- Prefer OAuth or GitHub App over long-lived user PATs.
- If PATs are unavoidable, use fine-grained, short-lived, least-privilege tokens.
- Use read-only mode first.
- Use narrow toolsets instead of `all`.
- Exclude dangerous tools explicitly when possible.
- Do not use repo/org-wide write scope unless the business case is strong and time-boxed.
- Confirm that logs show the user, token/app identity, tool/action, target resource, and write outcome.

## 6. Prohibited patterns

Reject or block MCP servers with any of these patterns unless a written exception exists:

```text
version = latest
unreviewed public package
classic PAT with broad repo/org scope
plaintext token in repository config
wildcard tool exposure
write/admin tools enabled by default
unclear server owner
unclear data handling
no server-side logs
local stdio server on Windows without endpoint controls
server starts directly from untrusted repo config
server can read arbitrary local files without isolation
server can send data to arbitrary external domains
```

## 7. Tool classification guide

| Tool type | Examples | Risk posture |
| --- | --- | --- |
| Read | get issue, search code, list PRs, read docs | Lower mutation risk, but data-exposure risk remains. |
| Write | create issue, comment, update PR, edit ticket | Requires explicit purpose and logs. |
| Execute | run shell, trigger workflow, start build, run query | High risk; validate runtime and credentials. |
| Admin | change permissions, secrets, branch rules, org settings | Critical; block by default. |
| External-send | post to chat, email, ticket, webhook, web request | High exfiltration risk. |
| Unknown | vague tool names or undocumented side effects | Treat as write/admin until proven otherwise. |

## 8. Logging schema

At minimum, MCP logs should capture:

| Field | Purpose |
| --- | --- |
| timestamp | Timeline reconstruction |
| user | Human identity behind the action |
| client_host | IDE/agent host identity |
| mcp_server | Server name and version |
| tool_name | Exact tool invoked |
| tool_class | Read/write/execute/admin/external-send |
| target_resource | Repo, issue, PR, API, database, object, or URL |
| auth_identity | OAuth app, GitHub App, service account, PAT class, or user token |
| request_id | Correlation across IDE, server, proxy, and target system |
| result | Success, failure, denied, partial |
| data_class | Public/internal/confidential/regulated/secrets-adjacent |
| bytes_or_record_count | Volume indicator where useful |
| policy_decision | Allowed, prompted, denied, exception |

Do not assume GitHub audit logs, IDE telemetry, or MCP server logs alone are enough. Design for correlation.

## 9. Reapproval and retirement

Re-review approved MCP servers when:

- The server version changes.
- The tool list changes.
- A write/admin tool is added.
- The authentication model changes.
- The owner changes.
- A dependency or package source changes.
- A security advisory affects the server or dependencies.
- The server has not been used for 90 days.
- The business use case no longer exists.

Retire a server by removing it from the registry/allowlist, revoking credentials, disabling secrets, archiving approval records, and documenting any downstream systems that must be cleaned up.
