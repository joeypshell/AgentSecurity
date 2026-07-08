# MCP Server Admission Record

Use this record before adding an MCP server to an approved registry, allowlist, custom agent, or pilot configuration.

## 1. Request metadata

| Field | Answer |
| --- | --- |
| Server name |  |
| Requestor |  |
| Server owner |  |
| Business owner |  |
| Security reviewer |  |
| Platform reviewer |  |
| Requested users/groups |  |
| Requested repositories/systems |  |
| Requested approval duration |  |
| Review date |  |

## 2. Business purpose

Describe the task this MCP server enables and why existing lower-risk tools are insufficient.

## 3. Server profile

| Field | Answer |
| --- | --- |
| Deployment type | Local stdio / Remote HTTP / SSE / Streamable HTTP / GitHub-hosted / Other |
| Hosting location | Developer workstation / Internal service / Vendor service / GitHub / Other |
| Source repository |  |
| Package/image/binary |  |
| Publisher/vendor |  |
| Version or digest |  |
| SBOM available | Y/N |
| Code signing/provenance available | Y/N |
| Update process |  |
| Rollback process |  |

## 4. Tool inventory

| Tool name | Description | Read/write/execute/admin/external-send | Inputs | Outputs | Target system | Data class | Side effects |
| --- | --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |  |

Attach full generated tool descriptions if available.

## 5. Data boundary

| Field | Answer |
| --- | --- |
| Allowed data sources |  |
| Allowed data sinks |  |
| Disallowed data |  |
| Can the server read local files? |  |
| Can the server read environment variables? |  |
| Can the server access developer credentials? |  |
| Can the server make outbound network calls? |  |
| Can the server write to external systems? |  |
| Can the server process regulated or customer data? |  |
| Data retention behavior |  |
| Data deletion process |  |

## 6. Authentication and authorization

| Field | Answer |
| --- | --- |
| Auth model | OAuth / GitHub App / Service account / Fine-grained PAT / Classic PAT / Local token / None |
| Required scopes/permissions |  |
| Token storage location |  |
| Token rotation period |  |
| Token owner |  |
| Per-user authorization enforced? |  |
| Resource/audience validation enforced? |  |
| PKCE/state/redirect validation, if OAuth |  |
| Least-privilege review complete? |  |

## 7. Logging and monitoring

| Field | Answer |
| --- | --- |
| Tool-call logs available |  |
| User identity captured |  |
| Tool name captured |  |
| Target resource captured |  |
| Read/write/admin distinction captured |  |
| Request/result captured |  |
| Prompt/tool content captured or redacted |  |
| SIEM integration |  |
| Correlation ID strategy |  |
| Alerting rules |  |
| Retention period |  |

## 8. Abuse cases

| Abuse case | Preventive control | Detective control | Residual risk |
| --- | --- | --- | --- |
| Prompt injection through tool output |  |  |  |
| Cross-tool exfiltration |  |  |  |
| Token leakage |  |  |  |
| Excessive write permissions |  |  |  |
| Local file/env access |  |  |  |
| Malicious package/update |  |  |  |
| Confused deputy/token audience failure |  |  |  |
| SSRF or arbitrary outbound call |  |  |  |

## 9. Test evidence

| Test | Result | Evidence link |
| --- | --- | --- |
| Tool inventory verified |  |  |
| Least-privilege auth verified |  |  |
| Read-only mode verified, if applicable |  |  |
| Write tools disabled or constrained |  |  |
| Prompt-injection test |  |  |
| Cross-tool exfiltration test |  |  |
| Logging reconstruction test |  |  |
| Token revocation test |  |  |
| Local file/env access test, if local |  |  |

## 10. Decision

- [ ] Approved baseline
- [ ] Approved pilot
- [ ] Approved exception
- [ ] Blocked
- [ ] Retired
- [ ] More evidence required

Conditions:

- 

Expiration/review date:

Approvers:

- Security Architecture:
- Platform Engineering:
- IAM:
- Detection Engineering:
- Legal/Privacy, if applicable:
- Business Owner:
