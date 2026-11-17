---
name: authentication-authorization
description: Implement authentication, sessions, permissions, RBAC, OAuth/OIDC, and secure identity flows.
---
# Authentication & Authorization

## Mission
Protect identity and resources with secure, explicit and testable authentication and authorization.

## Workflow
1. Identify actors, trust boundaries, session model, tenant model and sensitive actions.
2. Separate authentication (who is the caller?) from authorization (what may they do?).
3. Inspect existing token/session implementation and requirements.
4. Use vetted password hashing, secure cookie/token handling and validated OAuth/OIDC flows.
5. Define refresh rotation, revocation, expiry, replay detection and logout semantics.
6. Enforce resource-level and tenant-level permissions server-side.
7. Test unauthenticated, unauthorized, cross-tenant, expired, revoked and privilege-escalation cases.

## Rules
- Never trust role, user ID, tenant ID or permissions supplied by the client.
- Use least privilege and deny by default.
- Store secrets in managed secret storage, never source control.
- Protect against CSRF, XSS token theft, brute force and session fixation according to architecture.
- Validate OAuth state, nonce, PKCE and redirect URIs.

## Deliverables
Document threat assumptions, identity/session lifecycle, authorization matrix, revocation and security tests.
