# Agent Security Exception

Use this for time-boxed exceptions to the standard baseline, such as write-capable MCP tools, local stdio MCP, Visual Studio agent pilot gaps, preview models/features, sensitive-repo agent use, or temporary auto-approval changes.

## Exception summary

| Field | Answer |
| --- | --- |
| Exception title |  |
| Requestor |  |
| Business owner |  |
| Security owner |  |
| Capability class |  |
| Requested start date |  |
| Expiration date |  |
| Systems/repositories affected |  |
| Users/groups affected |  |

## Requested deviation

Describe the baseline control being bypassed or relaxed.

Examples:

- Enable write-capable GitHub MCP tool.
- Allow local stdio MCP on managed Windows workstation.
- Allow Visual Studio agent mode before full parity evidence exists.
- Allow agent mode on a high-sensitivity repository.
- Allow a preview model or feature.
- Temporarily permit broader network access.

## Business justification

What outcome is impossible or impractical without this exception?

## Risk analysis

| Risk | Impact | Likelihood | Control gap | Compensating control |
| --- | --- | --- | --- | --- |
|  |  |  |  |  |

## Required compensating controls

Check every control that applies.

- [ ] Named users only
- [ ] Named repositories only
- [ ] Managed devices only
- [ ] No local admin
- [ ] EDR process telemetry
- [ ] Proxy/DLP monitoring
- [ ] App control / allowlisted binary or package
- [ ] No production credentials available
- [ ] Branch protection enforced
- [ ] CODEOWNERS review required
- [ ] CI/status checks required
- [ ] Secret scanning enabled
- [ ] SAST/dependency review enabled
- [ ] MCP toolset narrowed
- [ ] Read-only mode used where possible
- [ ] Token scopes minimized
- [ ] Logs available to security
- [ ] Red-team validation complete
- [ ] Rollback plan documented
- [ ] Credential revocation plan documented

## Monitoring and alerting

| Signal | Source | Owner | Alert condition |
| --- | --- | --- | --- |
|  |  |  |  |

## Rollback plan

Describe how the exception will be disabled, credentials revoked, servers removed, settings restored, and changes reverted.

## Decision

- [ ] Approved
- [ ] Approved with conditions
- [ ] Blocked
- [ ] More evidence required

Conditions:

- 

Approvers:

- Security Architecture:
- Platform Engineering:
- Endpoint/Network, if applicable:
- IAM, if applicable:
- Legal/Privacy, if applicable:
- Business Owner:

Expiration date:

Review cadence:
