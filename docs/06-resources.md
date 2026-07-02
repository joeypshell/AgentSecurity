# Resources: Documentation, Videos, and Research

Last reviewed: 2026-07-02

Use official documentation as the source of truth for settings and feature behavior. VS Code, GitHub Copilot, MCP, and cloud-agent behavior can change quickly.

## 1. Official VS Code documentation

| Topic | Link |
| --- | --- |
| Build with agents in VS Code | https://code.visualstudio.com/docs/agents/overview |
| Agent concepts | https://code.visualstudio.com/docs/agents/concepts/agents |
| Local agents | https://code.visualstudio.com/docs/agents/agent-types/local-agents |
| Agent tools | https://code.visualstudio.com/docs/agents/agent-tools |
| Approvals and sandboxing | https://code.visualstudio.com/docs/agents/approvals |
| Security considerations | https://code.visualstudio.com/docs/agents/security |
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
| Content exclusion | https://docs.github.com/en/copilot/how-tos/configure-content-exclusion/exclude-content-from-copilot |
| Cloud agent firewall | https://docs.github.com/en/copilot/customizing-copilot/customizing-or-disabling-the-firewall-for-copilot-coding-agent |
| Cloud agent development environment customization | https://docs.github.com/en/copilot/using-github-copilot/coding-agent/customizing-the-development-environment-for-copilot-coding-agent |
| GitHub MCP Server repository | https://github.com/github/github-mcp-server |

## 3. Video watchlist

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

## 4. Security references

| Topic | Link |
| --- | --- |
| OWASP Top 10 for LLM Applications | https://genai.owasp.org/resource/owasp-top-10-for-llm-applications-2025/ |
| NIST AI Risk Management Framework | https://www.nist.gov/itl/ai-risk-management-framework |
| CISA Secure by Design | https://www.cisa.gov/securebydesign |
| GitHub secret scanning | https://docs.github.com/en/code-security/secret-scanning |
| GitHub dependency review | https://docs.github.com/en/code-security/supply-chain-security/understanding-your-software-supply-chain/about-dependency-review |
| GitHub branch protection | https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches |
| GitHub rulesets | https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets |

## 5. Research and security topics to track

Use these as search terms for current papers, talks, and advisories:

```text
LLM prompt injection
indirect prompt injection
AI coding agent malware
AI agent tool abuse
MCP server security
LLM data exfiltration
agentic coding security
AI-generated code vulnerabilities
software supply chain agent security
```

## 6. Terminology

| Term | Meaning |
| --- | --- |
| Agent | AI workflow that can plan and use tools toward a task |
| Tool | Callable capability such as read, edit, execute, web, MCP, or subagent invocation |
| MCP | Model Context Protocol, a way to connect AI tools to external systems and data |
| Prompt injection | Untrusted content attempts to override user or system intent |
| Handoff | User-visible transition from one agent to another |
| Subagent | Agent invoked by another agent for a scoped task |
| Sandbox | Runtime boundary restricting file, process, or network behavior |
| Content exclusion | Policy that excludes configured content from some Copilot features; do not assume it applies to every agent surface |

## 7. Maintenance notes

- Re-check official docs quarterly.
- Re-test custom agents after VS Code or Copilot updates.
- Re-review MCP servers after version changes.
- Revisit cloud-agent firewall and content-exclusion behavior after GitHub updates.
- Keep video links broad unless you have validated a specific video recently.
