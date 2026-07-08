# Enterprise Copilot and MCP Action Plan

Last reviewed: 2026-07-08

This plan turns agent-security research into an enterprise rollout sequence for GitHub Copilot, VS Code, Visual Studio, GitHub MCP Server, third-party MCP servers, and Copilot cloud/coding-agent workflows.

The central decision is simple: **approve normal Copilot use separately from agentic use**. Inline suggestions and standard chat are lower-autonomy assistance. Agent mode, cloud/coding agent, terminal execution, MCP tools, and write-capable integrations are delegated action paths and need their own approval gates.

## 1. Executive decision model

| Capability class | Examples | Default posture | Why |
| --- | --- | --- | --- |
| Class 1: suggestions | Copilot inline completions | Allow with baseline controls | Lowest autonomy; still subject to IP, privacy, and review controls. |
| Class 2: standard chat | Ask/explain/review without autonomous tool chains | Allow with baseline controls | Useful for discovery; easier to bound than agent mode. |
| Class 3: edit mode | IDE-assisted edits | Pilot only | Mutates code and may not inherit all chat controls. |
| Class 4: local agent mode | VS Code or Visual Studio agent that can read, edit, run commands, and call tools | Controlled pilot only | Crosses from recommendation into action. |
| Class 5: cloud/coding agent | GitHub-hosted issue-to-PR or task-to-PR agent | Low/medium-risk repo pilot only | Runs remotely and can produce repository changes. |
| Class 6: read-only MCP | GitHub MCP Server read-only/toolset-scoped | Small pilot only | Read-only can still expose sensitive data. |
| Class 7: write-capable MCP | GitHub/Jira/cloud/database tools that mutate state | Exception only | High blast radius and audit requirements. |
| Class 8: local/third-party MCP | stdio servers, public packages, community servers | Block by default | Treat as code execution plus API integration plus supply-chain risk. |

## 2. Non-negotiables

- **Do not equate Copilot approval with agent-mode approval.** Each higher-autonomy mode needs a separate control decision.
- **Do not treat content exclusion as a complete security boundary.** Use it where supported, but do not assume it protects agent mode, CLI, cloud agent, MCP-connected workflows, or indirect semantic leakage.
- **Disable global auto-approval by default.** Bypass approvals and autopilot-style behavior remove the human checkpoint that catches prompt injection and scope drift.
- **Default MCP to off.** Move only to approved registry, read-only, narrow-toolset configurations after admission review.
- **Block local stdio MCP on Windows by default.** If exceptions are required, require endpoint controls, no-local-admin posture, network egress filtering, credential isolation, and explicit owner approval.
- **Keep Visual Studio agent/MCP workflows pilot-only until control parity is proven.** Use external controls such as EDR, WDAC/AppLocker, proxy/DLP, branch protection, and required review to compensate.
- **Keep agent-generated changes inside normal SDLC controls.** Branch protection, CODEOWNERS, required reviews, CI, SAST, secret scanning, dependency review, deployment approvals, and human review remain mandatory.

## 3. First 30 days: stabilize the baseline

| Workstream | Owner | Actions | Deliverables |
| --- | --- | --- | --- |
| Governance | Security Architecture | Publish capability classes, interim policy, exception process, and pilot criteria. | Policy addendum, decision matrix, exception form. |
| Current-state discovery | Platform Engineering + GitHub Admins | Inventory Copilot users, IDE versions, VS Code settings, Visual Studio usage, existing MCP configs, extensions, tokens, repo sensitivity tiers, and current audit exports. | Inventory report and gap list. |
| GitHub baseline | GitHub Admins | Confirm enterprise-managed Copilot licensing, content exclusions for supported flows, audit streaming, public-code policy, model policy, and repo protections. | Settings evidence and SIEM integration status. |
| VS Code baseline | IDE Platform | Deploy managed settings: agent off outside pilot, global auto-approval off, MCP none by default, private registry only for pilots, extension tools off unless approved, network filtering, OTel export plan. | Managed settings package and validation evidence. |
| Endpoint/network baseline | Endpoint + Network Security | Validate no-local-admin posture, EDR process telemetry, proxy egress controls, DLP alerting, application control options, and workstation isolation patterns. | Compensating-control map. |

### 30-day exit criteria

- Capability classes are approved.
- Repositories are tiered by sensitivity.
- Agent mode is disabled or restricted outside named pilots.
- MCP is disabled by default.
- GitHub audit export is active or a gap is documented.
- VS Code managed settings have a deployment path.
- Local stdio MCP on Windows has an explicit default-deny decision.

## 4. Days 31-60: pilot VS Code agent mode without MCP

Start with VS Code because its enterprise control surface is the most concrete and policy-driven.

| Pilot dimension | Required value |
| --- | --- |
| Users | 10-25 experienced engineers |
| Repositories | Low/medium sensitivity only |
| Devices | Managed devices only |
| MCP | Disabled |
| Agent mode | Enabled only for pilot group |
| Global auto-approval | Disabled |
| Terminal commands | Manual approval required |
| Network access | Filtered and logged |
| Sandboxing | Enabled where supported |
| Logging | GitHub audit/activity, VS Code telemetry or OTel, EDR, proxy/DLP |
| SDLC | PR-based only; branch protection and required checks enforced |

### Pilot success criteria

- Pilot users cannot trivially bypass managed settings.
- Dangerous commands prompt or are blocked.
- Unapproved egress attempts are blocked or alerted.
- Sensitive files cannot be silently modified.
- Security can reconstruct meaningful activity from available evidence.
- Agent-created changes stay inside PR review and CI gates.
- Developers report useful outcomes without unsafe workarounds.

### Stop conditions

- Agent can silently run dangerous commands.
- Agent can modify CI/CD, IaC, auth, secrets, or deployment files without review.
- Prompt injection tests produce unsafe behavior without a detectable event.
- Audit reconstruction is not possible at a level acceptable to security operations.
- Developers can enable MCP or global auto-approval outside the pilot.

## 5. Days 31-60: build the MCP governance foundation

Do this in parallel with the no-MCP agent pilot.

| Action | Owner | Output |
| --- | --- | --- |
| Create MCP admission board | Security Architecture | Named approvers and review cadence |
| Define MCP risk classes | Security Architecture | Read-only, write-capable, local, remote, third-party, internal |
| Stand up private registry or allowlist process | Platform Engineering | Registry URL, allowlist, publishing process |
| Define token/auth standard | IAM + GitHub Admins | OAuth/GitHub App first; fine-grained PAT by exception; no broad classic PATs |
| Define logging schema | Detection Engineering | User, host, server, tool, target resource, read/write class, result class, correlation ID |
| Create intake templates | Security Architecture | MCP admission, agent pilot, exception, red-team test case |

## 6. Days 61-90: adversarial validation

Run tests in a lab with fake secrets, canary strings, representative repositories, fake internal APIs, and instrumented MCP servers. Do not run first-pass adversarial tests in production repositories.

Required scenarios:

1. Malicious README prompt injection.
2. Malicious repository instruction file.
3. Malicious prompt file.
4. Malicious GitHub issue/PR content.
5. Malicious build or test output.
6. Malicious package install script.
7. Terminal command escalation.
8. Agent modifies CI/CD workflow.
9. Agent modifies IaC or policy-as-code.
10. Agent accesses content-excluded files in an agentic flow.
11. MCP typosquat blocked by registry policy.
12. Local MCP attempts to read secrets, environment variables, SSH keys, and files outside the workspace.
13. Remote MCP confused-deputy or token-audience failure.
14. Cross-tool exfiltration from GitHub MCP to another SaaS MCP.
15. Audit reconstruction drill.
16. Visual Studio terminal and MCP behavior validation.

## 7. Days 91-120: add read-only GitHub MCP Server only if the pilot passed

Entry criteria:

- VS Code agent pilot passed.
- MCP registry/allowlist is live.
- MCP admission standard is approved.
- GitHub MCP Server configuration is read-only.
- Toolsets are narrowly scoped.
- Token scopes are minimized.
- GitHub audit, VS Code telemetry, MCP logs, endpoint telemetry, and proxy logs are correlated enough for incident response.

Allowed initial scope:

| Dimension | Recommendation |
| --- | --- |
| Server | Official GitHub MCP Server only |
| Mode | Read-only |
| Toolsets | Narrowest practical set |
| Users | Same or smaller group than VS Code agent pilot |
| Repositories | Low/medium sensitivity only |
| Write actions | Not allowed |
| Local MCP | Not allowed |
| Third-party MCP | Not allowed |

## 8. 120+ days: controlled expansion or explicit rejection

Broaden only when evidence supports it.

| Expansion gate | Required evidence |
| --- | --- |
| Policy enforceability | Managed settings deployed and verified across target population. |
| Auditability | Security can correlate GitHub audit/activity, IDE telemetry, endpoint logs, proxy logs, and MCP server logs. |
| SDLC integrity | Agent-generated changes cannot bypass branch protection, CODEOWNERS, required reviews, SAST, dependency review, secret scanning, CI, and deployment approvals. |
| MCP containment | Only approved registry servers are available; local stdio MCP remains blocked or tightly exceptioned. |
| Token safety | OAuth/GitHub App/fine-grained token patterns are validated; broad PATs are not used. |
| Incident readiness | Playbooks exist for prompt injection, MCP abuse, token exposure, and agent-generated vulnerable code. |
| Vendor blockers | Visual Studio parity, retention/residency behavior, and action-level audit questions are answered or risk-accepted. |

## 9. Operating model

| Body | Responsibility |
| --- | --- |
| AI Coding Assistant Governance Board | Approves capability classes, exceptions, and rollout gates. |
| Security Architecture | Owns threat model, control baseline, exception criteria, and residual-risk posture. |
| AppSec / Product Security | Owns red-team validation and secure SDLC integration. |
| Platform Engineering | Owns VS Code/Visual Studio configuration and developer experience. |
| GitHub Admins | Own enterprise/org policies, audit exports, repo controls, and GitHub MCP governance. |
| Endpoint Engineering | Own local admin restrictions, EDR, application control, and device posture. |
| Network Security | Own proxy, egress allowlists, DLP, and network telemetry. |
| IAM | Own OAuth/GitHub App/PAT/token policy. |
| Legal/Privacy | Reviews data handling, retention, residency, and regulated-data constraints. |
| Engineering Leadership | Accepts productivity/security tradeoffs. |

## 10. Monthly review package

During pilot, produce:

1. Enabled features by user group and repository.
2. Policy drift report.
3. Agent/MCP usage metrics.
4. Blocked command/tool/network events.
5. Red-team findings and remediation status.
6. Developer feedback.
7. Security incidents or near misses.
8. Exception list with expiration dates.
9. Vendor questions and open blockers.
10. Go/no-go recommendation for the next phase.
