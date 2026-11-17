---
name: security-review
description: Review application and AI-agent security, identify threats, and recommend mitigations. Use before releases and when adding auth, data flows, tools, or integrations.
---
# Security Review

## Mission
Find exploitable trust-boundary failures and prioritize practical mitigations without overstating assurance.

## Workflow
1. Identify assets, actors, trust boundaries, entry points and data flows.
2. Review authentication, authorization, tenant isolation, validation, secrets, dependencies and logging.
3. Check web risks: injection, XSS, CSRF, SSRF, IDOR, unsafe uploads and rate-limit gaps.
4. For AI systems, assess prompt injection, tool abuse, data exfiltration, excessive agency and insecure retrieval.
5. Verify controls in server-side code and tests; do not infer protection from UI restrictions.
6. Rank findings by impact, exploitability and exposure; provide safe reproduction conditions.
7. Recommend remediation and regression tests.

## Rules
- Never expose real secrets in reports.
- Do not run destructive or intrusive tests without authorization.
- Treat third-party content, uploaded files and model outputs as untrusted.
- Apply least privilege and defense in depth.
- Distinguish confirmed vulnerabilities from risks requiring validation.

## Deliverables
Provide scope, findings with severity and evidence, affected assets, remediation, regression tests and residual risk.
