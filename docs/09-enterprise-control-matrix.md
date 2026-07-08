# Enterprise Control Matrix for Copilot, Agents, and MCP

Last reviewed: 2026-07-08

Use this matrix to decide whether a Copilot, agent, or MCP capability is ready for broad rollout, pilot, exception-only use, or blocking.

## 1. Control-strength definitions

| Strength | Meaning | Example |
| --- | --- | --- |
| Hard | Centrally enforceable technical boundary that normal users cannot bypass. | Managed VS Code policy disabling global auto-approval. |
| Medium | Technical control that reduces risk but can be bypassed by alternate tools, unmanaged devices, or unsupported clients. | MCP registry restriction in VS Code when other MCP hosts remain allowed. |
| Soft | Guidance, prompt instruction, approval prompt, or review convention. | Developer asked not to paste secrets. |
| Evidence-only | Detective control without prevention. | Audit log showing a risky tool call after the fact. |
| Unknown | Product behavior or enforceability must be verified in the tenant or with vendor support. | Visual Studio parity for a specific agent/MCP control. |

## 2. Capability posture matrix

| Capability | Default posture | Required controls | Broad rollout blocker |
| --- | --- | --- | --- |
| Copilot inline suggestions | Allow with baseline | Enterprise license, approved org, content exclusions where useful, public-code/IP policy, SDLC review. | Individual/unmanaged accounts used for corporate code. |
| Standard Copilot Chat / Ask | Allow with baseline | Approved org, sensitive-data policy, content exclusions where supported, no agent tools outside pilot. | Chat used as a path to unmanaged tools or unapproved accounts. |
| Copilot Chat Edit | Pilot | Same as standard chat plus PR review, sensitive-file controls, and branch isolation. | Treated as equivalent to normal Ask mode. |
| VS Code agent mode | Controlled pilot | Managed settings, global auto-approval off, terminal approval, network filtering, sandboxing where supported, OTel/export plan, protected branches. | No central settings or no audit reconstruction. |
| Visual Studio agent mode | Pilot only | GitHub controls plus endpoint, EDR, no-local-admin, proxy/DLP, manual terminal review, PR controls. | Control parity with VS Code is unverified. |
| Copilot cloud/coding agent | Low/medium-risk repo pilot | Repo allowlist, branch protection, required reviews, Actions monitoring, limited secrets/environments. | Stakeholders assume content exclusion fully governs it. |
| GitHub MCP Server read-only | Small pilot | Private registry/allowlist, read-only URL/mode, narrow toolsets, minimal scopes, logging. | Toolset or auth cannot be narrowed enough. |
| Write-capable GitHub MCP | Exception only | Business owner, security sign-off, named users/repos, narrow tools, explicit logs, rollback plan. | No action-level monitoring or broad token scope. |
| Third-party remote MCP | Block until admitted | Provenance, owner, SBOM/signing where possible, narrow auth, logging, egress control, version pinning. | Opaque server or unclear data handling. |
| Local stdio MCP | Block by default on Windows | Endpoint controls, app control, no-local-admin, isolated environment, allowlisted binary/package, no secrets. | Runs arbitrary public package on host workstation. |
| Non-GitHub semantic indexing | Disabled by default | Explicit business case, policy approval, data-class review, deletion procedure. | Proprietary or regulated repo content uploaded without approval. |

## 3. GitHub controls

| Control | Recommended default | Strength | Owner | Evidence and monitoring | Residual risk |
| --- | --- | --- | --- | --- | --- |
| Enterprise-managed Copilot licensing | Required for corporate development | Hard if enforced through identity and procurement | GitHub Admins + IAM | Copilot seat list, SSO/SCIM records | Users may use personal tools outside managed environment. |
| Individual Copilot plans for corporate code | Prohibited | Medium | Security + Legal + Engineering | Policy attestation, proxy/endpoint discovery | Hard to detect on unmanaged devices. |
| Content exclusions | Enable for sensitive paths where supported | Medium/soft | GitHub Admins + Repo Owners | GitHub policy export, test evidence | Not universal; does not replace repo/data controls. |
| Public-code matching/IP policy | Enable block or annotation based on legal posture | Medium | Legal + GitHub Admins | Policy screenshots, PR review records | Similar code can still be generated without exact match. |
| Model allowlist | GA approved models only; previews by exception | Medium | Security + Legal/Privacy | Copilot policy and vendor data-handling evidence | Model routing and retention behavior may change. |
| GitHub audit log streaming | Required | Evidence-only | GitHub Admins + Detection Engineering | SIEM ingestion, retention tests | Does not capture local IDE prompt/session details. |
| Copilot activity reporting | Required for usage oversight | Evidence-only | GitHub Admins | Activity reports, trend dashboards | Not sufficient for action-level MCP reconstruction. |
| Branch protection/rulesets | Required on protected repos | Hard | GitHub Admins + Repo Owners | Ruleset export, PR evidence | Bypass roles must be tightly controlled. |
| CODEOWNERS | Required for sensitive paths | Medium/hard depending on ruleset | Repo Owners | CODEOWNERS plus required review evidence | Wrong owners or stale patterns weaken the control. |

## 4. VS Code controls

| Control | Recommended default | Strength | Owner | Evidence and monitoring | Residual risk |
| --- | --- | --- | --- | --- | --- |
| Agent mode enablement | Disabled outside pilot | Hard on managed devices | IDE Platform | MDM/server-managed/file policy evidence | Unmanaged editors may not honor policy. |
| Global auto-approval / Autopilot / Bypass Approvals | Disabled | Hard on managed devices | IDE Platform | Policy evidence, UI validation | Other clients may expose similar behavior differently. |
| Terminal auto-approval | Disabled or very narrow | Medium | IDE Platform + Security | Policy, OTel, EDR command telemetry | Command parsing and prompt injection remain hard. |
| Sensitive-file auto-edit rules | Block silent edits to secrets, CI/CD, IaC, auth, deployment, agent config | Medium | IDE Platform + AppSec | Settings evidence, PR review | Pattern coverage can miss files. |
| MCP access | `none` by default, `registry` in pilot | Hard/medium | IDE Platform | Policy evidence, registry logs | Does not govern third-party hosts outside VS Code. |
| MCP registry URL | Private curated registry | Medium | Platform Engineering | Registry config and publish logs | Registry process quality becomes the control. |
| Extension-contributed tools | Disabled unless reviewed | Medium | IDE Platform | Extension inventory and settings | Extensions can still be installed if marketplace not governed. |
| Hooks | Disabled by default | Medium | IDE Platform | Settings evidence | Hooks run local commands if allowed. |
| Network filtering | Enabled for agent/browser/fetch/sandboxed flows | Medium | Network + IDE Platform | Proxy logs, deny events, settings | Does not cover every local process without endpoint controls. |
| Sandboxing | Enabled where supported | Medium | IDE Platform + Endpoint | Settings, lab validation | Coverage differs by OS and tool path. |
| Workspace Trust | Required for untrusted repos | Medium | IDE Platform | Settings, developer workflow tests | Malicious extensions may not honor restricted mode. |
| OpenTelemetry export | Enable to approved collector; decide content capture policy | Evidence-only | Detection Engineering + Privacy | Collector logs, schema docs | Capturing prompts/tool results can create privacy and retention issues. |

## 5. Visual Studio controls

| Control | Recommended default | Strength | Owner | Evidence and monitoring | Residual risk |
| --- | --- | --- | --- | --- | --- |
| Agent mode | Pilot-only | Unknown/medium | IDE Platform | Tenant test evidence, vendor confirmation | Public control parity with VS Code may be incomplete. |
| MCP tools | Disabled until verified | Unknown/medium | IDE Platform + Security | Pilot evidence, vendor docs | Governance may not match VS Code. |
| Terminal command review | Manual approval required | Soft/medium | Developers + Endpoint | EDR process telemetry, user prompts | Running Visual Studio elevated increases blast radius. |
| Running elevated | Prohibited for normal agent work | Medium | Endpoint + Developer Experience | EDR, workstation policy | Local admin exceptions remain dangerous. |
| Endpoint compensation | EDR, WDAC/AppLocker, no-local-admin, proxy/DLP | Medium | Endpoint + Network | Telemetry and policy reports | Compensates but does not equal IDE-native policy. |

## 6. MCP controls

| Control | Recommended default | Strength | Owner | Evidence and monitoring | Residual risk |
| --- | --- | --- | --- | --- | --- |
| MCP default posture | Disabled | Hard/medium | Security + IDE Platform | VS Code policy, registry logs | Other AI clients may still be used. |
| MCP admission review | Required before enablement | Medium | Security Architecture | Approved intake record | Review can become rubber-stamp without testing. |
| Private registry/allowlist | Required for pilots | Medium/hard | Platform Engineering | Registry config and server list | Does not solve runtime compromise. |
| Read-only mode | Required first | Medium | MCP Owner + Security | Server config and test evidence | Read-only can still expose sensitive data. |
| Toolset scoping | Narrowest practical set | Medium | MCP Owner | Tool inventory and config | Poorly named tools may hide write behavior. |
| Token model | OAuth/GitHub App first; fine-grained PAT by exception | Medium | IAM + GitHub Admins | Scope review, token inventory | Human tokens can still widen blast radius. |
| Version pinning | Required | Medium | MCP Owner | Lockfile, digest, package metadata | Pinned malicious version is still malicious. |
| Provenance/SBOM/signing | Required where available | Medium | Platform Engineering | SBOM, signature, source repo | Many MCP servers may not support this. |
| Server-side logging | Required | Evidence-only | MCP Owner + Detection | Tool-call logs | Logs may omit prompt/tool content. |
| Local stdio MCP on Windows | Block by default | Hard/medium | Endpoint + IDE Platform | Policy, app control, EDR | Exceptions require strong isolation. |

## 7. SDLC controls

| Control | Recommended default | Strength | Owner | Evidence and monitoring | Residual risk |
| --- | --- | --- | --- | --- | --- |
| PR-only changes | Required for agent-authored code | Hard/medium | Repo Owners | PR evidence | Direct pushes must be blocked. |
| Required reviews | Required | Hard/medium | Repo Owners | Branch rules and PR approvals | Review quality varies. |
| Security-sensitive CODEOWNERS | Required | Medium | Repo Owners + AppSec | CODEOWNERS and review requests | Ownership drift. |
| SAST/secret/dependency scanning | Required | Medium | AppSec | CI artifacts and alerts | Tools miss logic flaws and prompt-induced scope drift. |
| CI/CD workflow review | Required | Medium | Platform + AppSec | PR diffs and CODEOWNERS | Agent can make subtle workflow changes. |
| Deployment approvals | Required for protected environments | Hard | Platform + Release Engineering | Deployment logs | Admin bypass roles remain sensitive. |
| Agent-use disclosure in PRs | Required for substantial changes | Soft/evidence | Developers + Reviewers | PR template fields | User-dependent unless automated. |

## 8. Endpoint, identity, and network controls

| Control | Recommended default | Strength | Owner | Evidence and monitoring | Residual risk |
| --- | --- | --- | --- | --- | --- |
| No local admin | Default for developer workstations | Medium | Endpoint | Device compliance | Some dev roles may need exceptions. |
| EDR process telemetry | Required | Evidence-only/medium | Endpoint + Detection | Command/process/network logs | Does not prevent every bad action. |
| Application control | Required for MCP exceptions | Hard/medium | Endpoint | WDAC/AppLocker policy evidence | Operational overhead. |
| Proxy egress allowlists | Required for pilots | Medium | Network | Proxy logs and deny events | Local processes can use alternate paths if not controlled. |
| DLP | Required for sensitive repos | Evidence/medium | Network + Data Security | Alerts and canary tests | DLP may not understand source context. |
| Secret storage | No plaintext tokens in MCP or repo config | Medium | IAM + Platform | Secret scans, config review | Tokens may live in local credential stores. |
| Token rotation | Required for exceptions | Medium | IAM | Rotation records | Rotating does not fix over-scoping. |

## 9. Decision rule

Use the strictest relevant control category. For example:

- A read-only GitHub MCP Server used by VS Code agent mode is governed by **VS Code agent controls**, **GitHub MCP controls**, **token controls**, **MCP logging**, and **SDLC controls**.
- A Visual Studio agent that can run terminal commands is governed by **Visual Studio controls**, **endpoint controls**, **terminal controls**, and **SDLC controls**.
- A third-party local stdio MCP server is governed by **MCP admission**, **supply-chain review**, **endpoint/app control**, **credential isolation**, and **network egress**.

If the required evidence cannot be produced, keep the capability in pilot or blocked status.
