# Resources: Documentation, Videos, and Research

Last reviewed: 2026-07-08

Use official documentation as the source of truth for settings and feature behavior. VS Code, Visual Studio, GitHub Copilot, MCP, cloud/coding-agent behavior, model routing, data retention, and audit coverage can change quickly.

## 1. Official VS Code documentation

| Topic | Link |
| --- | --- |
| Build with agents in VS Code | https://code.visualstudio.com/docs/agents/overview |
| Agent concepts | https://code.visualstudio.com/docs/agents/concepts/agents |
| Local agents | https://code.visualstudio.com/docs/agents/agent-types/local-agents |
| Agent tools | https://code.visualstudio.com/docs/agents/agent-tools |
| Approvals and sandboxing | https://code.visualstudio.com/docs/agents/approvals |
| Security considerations | https://code.visualstudio.com/docs/agents/security |
| Enterprise AI settings | https://code.visualstudio.com/docs/enterprise/ai-settings |
| MCP servers in VS Code | https://code.visualstudio.com/docs/agent-customization/mcp-servers |
| Custom agents | https://code.visualstudio.com/docs/agent-customization/custom-agents |
| Subagents | https://code.visualstudio.com/docs/agents/subagents |
| Custom instructions | https://code.visualstudio.com/docs/copilot/customization/custom-instructions |
| Prompt files | https://code.visualstudio.com/docs/copilot/customization/prompt-files |
| Agent skills | https://code.visualstudio.com/docs/agent-customization/agent-skills |

## 2. Official GitHub Copilot documentation

| Topic | Link |
| --- | --- |
| Copilot cloud agent overview | https://docs.github.com/en/copilot/concepts/agents/cloud-agent/about-cloud-agent |
| Custom agents configuration | https://docs.github.com/en/copilot/reference/custom-agents-configuration |
| MCP in GitHub Copilot | https://docs.github.com/en/copilot/concepts/context/mcp |
| Configure MCP server access | https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-mcp-usage/configure-mcp-server-access |
| MCP in your IDE | https://docs.github.com/en/copilot/how-tos/provide-context/use-mcp-in-your-ide/extend-copilot-chat-with-mcp |
| Content exclusion | https://docs.github.com/en/copilot/how-tos/configure-content-exclusion/exclude-content-from-copilot |
| Repository indexing for Copilot Chat | https://docs.github.com/en/copilot/concepts/indexing-repositories-for-copilot-chat |
| Supported AI models in Copilot | https://docs.github.com/en/copilot/reference/ai-models/supported-models |
| Model hosting for Copilot | https://docs.github.com/en/copilot/reference/ai-models/model-hosting |
| Copilot metrics data | https://docs.github.com/en/copilot/reference/metrics-data |
| Cloud agent firewall | https://docs.github.com/en/copilot/customizing-copilot/customizing-or-disabling-the-firewall-for-copilot-coding-agent |
| Cloud agent development environment customization | https://docs.github.com/en/copilot/using-github-copilot/coding-agent/customizing-the-development-environment-for-copilot-coding-agent |
| GitHub MCP Server repository | https://github.com/github/github-mcp-server |
| GitHub MCP Server configuration docs | https://github.com/github/github-mcp-server/tree/main/docs |

## 3. Official Visual Studio / Microsoft documentation

| Topic | Link |
| --- | --- |
| Visual Studio Copilot documentation | https://learn.microsoft.com/en-us/visualstudio/ide/visual-studio-github-copilot-extension |
| Visual Studio Copilot agent mode | https://learn.microsoft.com/en-us/visualstudio/ide/copilot-agent-mode |
| Microsoft security documentation | https://learn.microsoft.com/en-us/security/ |

Visual Studio control parity with VS Code should be verified directly in your tenant and with current Microsoft/GitHub documentation before broad rollout.

## 4. MCP specification and security guidance

| Topic | Link |
| --- | --- |
| Model Context Protocol | https://modelcontextprotocol.io/ |
| MCP specification | https://modelcontextprotocol.io/specification/ |
| MCP security best practices | https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices |
| MCP authorization | https://modelcontextprotocol.io/specification/latest/basic/authorization |

## 5. Video watchlist

Video titles and product UI change often. Prefer official channels and current search results over hard-coded old videos.

| What to watch for | Link |
| --- | --- |
| Official VS Code channel | https://www.youtube.com/@code |
| Official GitHub channel | https://www.youtube.com/@GitHub |
| Microsoft Developer channel | https://www.youtube.com/@MicrosoftDeveloper |
| Search: VS Code Copilot agent mode | https://www.youtube.com/results?search_query=VS+Code+Copilot+agent+mode |
| Search: VS Code Copilot custom agents | https://www.youtube.com/results?search_query=VS+Code+Copilot+custom+agents |
| Search: GitHub Copilot coding agent | https://www.youtube.com/results?search_query=GitHub+Copilot+coding+agent |
| Search: GitHub Copilot MCP VS Code | https://www.youtube.com/results?search_query=GitHub+Copilot+MCP+VS+Code |
| Search: AI coding agent security prompt injection | https://www.youtube.com/results?search_query=AI+coding+agent+security+prompt+injection |
| Search: OWASP LLM prompt injection agents | https://www.youtube.com/results?search_query=OWASP+LLM+prompt+injection+agents |

Recommended viewing order:

1. VS Code Copilot agent mode overview.
2. Custom agents and prompt files.
3. MCP server setup and governance.
4. Copilot coding agent or cloud agent workflows.
5. Prompt injection and LLM application security talks.

## 6. Security references

| Topic | Link |
| --- | --- |
| OWASP Top 10 for LLM Applications | https://owasp.org/www-project-top-10-for-large-language-model-applications/ |
| OWASP Gen AI Security Project | https://genai.owasp.org/ |
| OWASP Agentic AI security resources | https://genai.owasp.org/ |
| NIST AI Risk Management Framework | https://www.nist.gov/itl/ai-risk-management-framework |
| NIST Generative AI Profile | https://www.nist.gov/itl/ai-risk-management-framework |
| Cloud Security Alliance AI Controls Matrix | https://cloudsecurityalliance.org/artifacts/ai-controls-matrix-v1-1 |
| CISA Secure by Design | https://www.cisa.gov/securebydesign |
| GitHub secret scanning | https://docs.github.com/en/code-security/secret-scanning |
| GitHub dependency review | https://docs.github.com/en/code-security/supply-chain-security/understanding-your-software-supply-chain/about-dependency-review |
| GitHub branch protection | https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches |
| GitHub rulesets | https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets |

## 7. Research and security topics to track

Use these as search terms for current papers, talks, and advisories:

```text
LLM prompt injection
indirect prompt injection
AI coding agent malware
AI agent tool abuse
MCP server security
MCP confused deputy
MCP token passthrough
MCP SSRF
MCP session hijacking
LLM data exfiltration
agentic coding security
AI-generated code vulnerabilities
software supply chain agent security
Copilot cloud agent security
VS Code agent mode enterprise settings
Visual Studio Copilot agent mode enterprise controls
```

## 8. Terminology

| Term | Meaning |
| --- | --- |
| Agent | AI workflow that can plan and use tools toward a task. |
| Tool | Callable capability such as read, edit, execute, web, MCP, or subagent invocation. |
| MCP | Model Context Protocol, a way to connect AI tools to external systems and data. |
| Prompt injection | Untrusted content attempts to override user or system intent. |
| Tool-output injection | Tool response attempts to steer later model/tool behavior. |
| Handoff | User-visible transition from one agent to another. |
| Subagent | Agent invoked by another agent for a scoped task. |
| Sandbox | Runtime boundary restricting file, process, or network behavior. |
| Content exclusion | Policy that excludes configured content from some Copilot features; do not assume it applies to every agent surface. |
| Read-only MCP | MCP configuration that prevents writes but can still read and expose sensitive data. |
| Write-capable MCP | MCP configuration that can mutate repositories, tickets, cloud resources, databases, or other systems. |

## 9. Maintenance notes

- Re-check official docs quarterly.
- Re-test custom agents after VS Code, Visual Studio, or Copilot updates.
- Re-review MCP servers after version, tool, auth, or owner changes.
- Revisit cloud-agent firewall and content-exclusion behavior after GitHub updates.
- Revisit model/data-retention behavior after GitHub model-provider or preview-feature changes.
- Keep video links broad unless you have validated a specific video recently.
