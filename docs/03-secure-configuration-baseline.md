# Secure Configuration Baseline

Last reviewed: 2026-07-02

This baseline is intentionally conservative. Relax it only when the repository, agent, tools, and credentials are trusted for the specific task.

## 1. Local VS Code baseline

Start from this posture:

```jsonc
{
  // Require deliberate approval for terminal commands.
  "chat.tools.terminal.enableAutoApprove": false,

  // Prefer OS-level sandboxing for agent-run terminal commands.
  // macOS/Linux:
  "chat.agent.sandbox.enabled": "on",

  // Windows:
  "chat.agent.sandbox.enabledWindows": "on",

  // Restrict network access for sandboxed commands and agent tools.
  "chat.agent.networkFilter": true,
  "chat.agent.allowedNetworkDomains": [
    "github.com",
    "api.github.com"
  ],
  "chat.agent.deniedNetworkDomains": [],

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
    "**/infra/**": false,
    "**/terraform/**": false
  }
}
```

Copy `.vscode/settings.example.jsonc` into local settings only after adapting it to your environment.

## 2. Permission-level guidance

| Permission posture | Use case | Security note |
| --- | --- | --- |
| Default approvals | Normal work | Best default for real repositories |
| Session-scoped auto-approval | Repetitive low-risk commands | Keep it temporary and narrow |
| Bypass approvals | Disposable sandbox only | Unsafe for normal development |
| Autopilot-style behavior | Narrow experiment only | Avoid for repos with credentials or cloud CLIs |

Do not confuse productivity with safety. Reduced approval friction means fewer chances to catch prompt injection or scope drift.

## 3. Terminal command policy

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

## 4. Sensitive path policy

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
infra/**
terraform/**
k8s/**
charts/**
Dockerfile
compose*.yml
package manager lockfiles
```

Lockfiles are not always high risk, but they are supply-chain artifacts. They deserve review when an agent changes them.

## 5. Repository governance baseline

Minimum repository controls:

- protected default branch
- required pull request review
- required status checks
- CODEOWNERS on agent, prompt, workflow, IaC, and security-sensitive files
- secret scanning enabled where available
- dependency scanning enabled where available
- no long-lived production credentials in repository or local workspace
- PR template requiring agent-use disclosure for substantial agent-authored changes

Suggested CODEOWNERS entries are included in `.github/CODEOWNERS`.

## 6. Custom agent governance

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

## 7. MCP baseline

Default MCP position: deny by default, approve by exception.

For each MCP server, document:

| Field | Required answer |
| --- | --- |
| Owner | Who maintains it |
| Source | Package, container image, repo, or internal build |
| Version pin | Exact version or digest |
| Tools exposed | Full list |
| Read/write capability | Which tools mutate data |
| Credential type | Service account, OAuth, local token, none |
| Scope | Least-privilege scopes |
| Logging | Where calls are recorded |
| Update process | Who approves updates |

Do not let an agent discover and install MCP servers opportunistically in a sensitive repo.

## 8. Cloud agent baseline

For cloud-based coding agents:

- create tasks from issues with clear scope
- keep repository secrets unavailable unless required
- use protected branches
- require human review on generated PRs
- require status checks
- review changes to Actions workflows, dependencies, IaC, auth, and secrets especially carefully
- do not rely on network firewalling or content exclusion as complete data-loss prevention
- avoid connecting broad write-capable MCP servers

## 9. Workstation isolation

Recommended operating pattern:

```text
trusted repo + low-risk read-only review -> normal workspace
trusted repo + agent edits -> branch or worktree
untrusted repo + tests/builds -> devcontainer or disposable VM
untrusted repo + package install scripts -> do not run locally
cloud/IaC mutation -> CI/CD pipeline with approval gates
```

## 10. Review expectations

Every substantial agent-authored change should include:

- summary of requested task
- summary of files changed
- tests run and results
- known failures or skipped checks
- security-sensitive areas touched
- dependency or lockfile changes
- evidence for authorization and input-validation behavior
- reviewer notes for residual risk

If the agent cannot produce evidence, treat its conclusion as a hypothesis, not a finding.
