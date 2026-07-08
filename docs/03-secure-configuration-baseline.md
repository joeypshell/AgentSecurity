# Secure Configuration Baseline

Last reviewed: 2026-07-08

This baseline is intentionally conservative. Relax it only when the repository, agent, tools, credentials, and telemetry are trusted for the specific task.

The most important enterprise split is:

```text
inline suggestions / standard chat != edit mode != agent mode != cloud agent != MCP-enabled agent
```

Do not inherit approval from a lower-autonomy mode into a higher-autonomy mode.

## 1. Enterprise default posture

| Capability | Recommended default |
| --- | --- |
| Copilot inline suggestions | Allow with enterprise-managed accounts, public-code/IP policy, and normal SDLC review. |
| Standard Copilot Chat / Ask | Allow with enterprise-managed accounts, sensitive-data guidance, and supported content exclusions. |
| Copilot Chat Edit | Pilot only. |
| VS Code agent mode | Pilot only; managed devices and low/medium-risk repos first. |
| Visual Studio agent mode | Pilot only until control parity, terminal behavior, MCP governance, and telemetry are validated. |
| Copilot cloud/coding agent | Low/medium-risk repo pilot only; use PR review and branch protections. |
| MCP | Disabled by default; approved registry/allowlist for pilot only. |
| GitHub MCP Server | Read-only, narrow toolset pilot first. |
| Third-party MCP | Block by default. |
| Local stdio MCP | Block by default on Windows developer workstations. |
| Write-capable MCP | Exception only. |
| Preview models/features | Disabled unless explicitly approved. |

## 2. Local VS Code example settings

Use `.vscode/settings.example.jsonc` as a starting point for local experiments. For enterprise rollout, prefer managed settings/policy through MDM, server-managed settings, or another central mechanism.

```jsonc
{
  // Treat agent mode as a pilot capability.
  "chat.agent.enabled": false,

  // Do not allow global tool auto-approval, Bypass Approvals, or Autopilot-style behavior by default.
  "chat.tools.global.autoApprove": false,

  // Require deliberate approval for terminal commands.
  "chat.tools.terminal.enableAutoApprove": false,

  // Prefer OS-level sandboxing for agent-run terminal commands where supported.
  "chat.agent.sandbox.enabled": "on",
  "chat.agent.sandbox.enabledWindows": "on",

  // Restrict network access for sandboxed commands and agent tools.
  "chat.agent.networkFilter": true,
  "chat.agent.allowedNetworkDomains": [
    "github.com",
    "api.github.com"
  ],
  "chat.agent.deniedNetworkDomains": [],

  // Default MCP to off. Move to registry/allowlist only after formal admission review.
  "chat.mcp.access": "none",

  // Disable extension-contributed tools and hooks unless reviewed.
  "chat.extensionTools.enabled": false,
  "chat.useHooks": false,

  // Browser/fetch/web tools are indirect prompt-injection surfaces.
  "chat.tools.browser.enabled": false,

  // Sensitive files should not be silently modified.
  "chat.tools.edits.autoApprove": {
    "**/.env": false,
    "**/.env.*": false,
    "**/*secret*": false,
    "**/*credential*": false,
    "**/*.pem": false,
    "**/*.key": false,
    "**/*.p12": false,
    "**/*.pfx": false,
    "**/.github/workflows/**": false,
    "**/.github/agents/**": false,
    "**/.github/prompts/**": false,
    "**/.github/copilot-instructions.md": false,
    "**/AGENTS.md": false,
    "**/infra/**": false,
    "**/terraform/**": false,
    "**/k8s/**": false,
    "**/charts/**": false,
    "**/Dockerfile": false,
    "**/compose*.yml": false,
    "**/package-lock.json": false,
    "**/pnpm-lock.yaml": false,
    "**/yarn.lock": false,
    "**/poetry.lock": false,
    "**/requirements*.txt": false
  }
}
```

Settings change quickly. Verify names and supported values against your current VS Code and Copilot release before enforcing them.

## 3. Enterprise VS Code managed baseline

| Control | Recommended value | Notes |
| --- | --- | --- |
| Agent mode | Disabled outside named pilots | Separate from standard chat approval. |
| Global auto-approval | Disabled | Prevents broad bypass of approval prompts. |
| Terminal auto-approval | Disabled or narrow allowlist | Treat command filters as helpful, not perfect. |
| MCP access | `none` generally; `registry` for pilot | Never `all` as enterprise default. |
| MCP registry | Private curated registry | Review server owner, tools, version, auth, logs, and data class. |
| Extension tools | Disabled unless reviewed | Extensions can add powerful agent tools. |
| Hooks | Disabled by default | Hooks can run local commands and should be reviewed like code. |
| Browser/fetch tools | Disabled or domain-filtered | Web content can be prompt-injection input. |
| Network filtering | Enabled for pilots | Pair with proxy logs and DLP where possible. |
| Sandboxing | Enabled where supported | Validate actual behavior by OS and tool path. |
| Workspace Trust | Required for untrusted repos | Restricted Mode is a first boundary, not a complete containment system. |
| OTel/telemetry | Export to approved collector if allowed | Decide whether prompt/tool content capture is allowed under privacy policy. |

## 4. Permission-level guidance

| Permission posture | Use case | Security note |
| --- | --- | --- |
| Default approvals | Normal work | Best default for real repositories. |
| Session-scoped auto-approval | Repetitive low-risk commands | Keep it temporary and narrow. |
| Bypass approvals | Disposable sandbox only | Unsafe for normal development. |
| Autopilot-style behavior | Narrow experiment only | Avoid for repos with credentials, cloud CLIs, production config, or MCP. |

Do not confuse productivity with safety. Reduced approval friction means fewer chances to catch prompt injection, tool-output injection, or scope drift.

## 5. Terminal command policy

Prefer commands that read, test, or validate:

```text
git status
git diff
npm test
pytest
mvn test
dotnet test
go test ./...
terraform fmt -check
terraform validate
terraform plan
```

Treat these as high risk:

```text
curl ... | sh
wget ... | bash
Invoke-Expression
npm install from untrusted repo
pip install from untrusted repo
python setup.py install
terraform apply
kubectl apply
helm upgrade
az deployment group create
aws iam create-role
gh secret set
rm -rf
```

For cloud and infrastructure work, prefer a pipeline with review gates over workstation execution.

## 6. Sensitive path policy

Require explicit human review for any agent change touching:

```text
.env
.env.*
*.pem
*.key
*.p12
*.pfx
.github/workflows/**
.github/agents/**
.github/prompts/**
.github/copilot-instructions.md
AGENTS.md
.vscode/**
infra/**
terraform/**
k8s/**
charts/**
Dockerfile
compose*.yml
package manager lockfiles
```

Lockfiles are not always high risk, but they are supply-chain artifacts. They deserve review when an agent changes them.

## 7. Repository governance baseline

Minimum repository controls:

- Protected default branch.
- Required pull request review.
- Required status checks.
- CODEOWNERS on agent, prompt, workflow, IaC, settings, templates, and security-sensitive files.
- Secret scanning enabled where available.
- Dependency scanning enabled where available.
- SAST or code scanning enabled where available.
- No long-lived production credentials in repository or local workspace.
- PR template requiring agent/Copilot/MCP disclosure for substantial agent-authored changes.
- Review rules for CI/CD, IaC, auth, secrets, deployment, and policy-as-code.

Suggested CODEOWNERS entries are included in `.github/CODEOWNERS`.

## 8. Custom agent governance

Every custom agent should answer these questions:

1. Why does this role exist?
2. What can it read?
3. What can it edit?
4. What can it execute?
5. Which MCP servers can it call?
6. Which subagents can it invoke?
7. What must always require human approval?
8. What files must it never modify?
9. What evidence must it produce?
10. How will abuse be detected?

Reject or quarantine custom agents with:

```text
tools: ['*']
agents: ['*']
unclear descriptions
instructions to ignore policy
hidden external URLs
write tools for review-only roles
production cloud mutation permissions
```

## 9. MCP baseline

Default MCP position: deny by default, approve by exception.

Before enabling a server, complete `templates/mcp-server-admission.md` and review `docs/10-mcp-server-admission-standard.md`.

For each MCP server, document:

| Field | Required answer |
| --- | --- |
| Owner | Who maintains it. |
| Source | Package, container image, repo, internal build, or vendor endpoint. |
| Version pin | Exact version or digest. |
| Tools exposed | Full list. |
| Read/write capability | Which tools mutate data. |
| Credential type | OAuth, GitHub App, fine-grained PAT, service account, local token, none. |
| Scope | Least-privilege scopes and resource boundaries. |
| Logging | Where calls are recorded and what fields are captured. |
| Update process | Who approves updates. |
| Expiration | When the approval is reviewed or retired. |

Do not let an agent discover and install MCP servers opportunistically in a sensitive repo.

## 10. Cloud/coding-agent baseline

For cloud-based coding agents:

- Create tasks from issues with clear scope.
- Keep repository secrets unavailable unless required.
- Use protected branches.
- Require human review on generated PRs.
- Require status checks.
- Review changes to Actions workflows, dependencies, IaC, auth, and secrets especially carefully.
- Do not rely on network firewalling or content exclusion as complete data-loss prevention.
- Avoid connecting broad write-capable MCP servers.
- Keep sensitive and crown-jewel repositories out of pilot until controls are tested.

## 11. Visual Studio baseline

Until enterprise control parity is proven in your tenant:

- Keep Visual Studio agent mode pilot-only.
- Require manual terminal/tool approval.
- Do not run Visual Studio elevated for ordinary agent work.
- Use managed devices only.
- Require EDR process telemetry.
- Use application control where feasible.
- Use proxy/DLP for egress visibility.
- Keep MCP disabled by default.
- Apply PR review, branch protection, CODEOWNERS, and CI checks just as strictly as VS Code workflows.

## 12. Workstation isolation

Recommended operating pattern:

```text
trusted repo + low-risk read-only review -> normal workspace
trusted repo + agent edits -> branch or worktree
untrusted repo + tests/builds -> devcontainer or disposable VM
untrusted repo + package install scripts -> do not run locally
cloud/IaC mutation -> CI/CD pipeline with approval gates
local stdio MCP on Windows -> blocked unless exceptioned with endpoint controls
```

## 13. Review expectations

Every substantial agent-authored change should include:

- Summary of requested task.
- Summary of files changed.
- Agent/Copilot/MCP disclosure.
- Tests run and results.
- Known failures or skipped checks.
- Security-sensitive areas touched.
- Dependency or lockfile changes.
- Evidence for authorization and input-validation behavior.
- Reviewer notes for residual risk.

If the agent cannot produce evidence, treat its conclusion as a hypothesis, not a finding.
