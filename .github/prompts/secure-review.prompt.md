---
description: Perform a read-only security review of the current diff or selected scope.
agent: Security Reviewer
---

# Secure review prompt

Review the selected scope as untrusted agent-authored or agent-assisted code.

Focus on:

- authentication and authorization
- tenant isolation
- injection
- SSRF
- path traversal
- unsafe file handling
- unsafe deserialization
- insecure crypto
- token and secret exposure
- sensitive logging
- dependency and supply-chain risk
- CI/CD or policy weakening
- missing negative tests

For each finding, return severity, evidence, exploit scenario, recommended fix, tests, and confidence.

Do not edit files. Do not run commands.
