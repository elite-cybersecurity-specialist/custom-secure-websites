# ECS — Security Embedded in Website Source Code

**Foundation 2: build defensive controls into the application itself.** This is a technology-neutral requirements checklist; platform/framework-specific implementation is selected for each customer.

[← Back to README](../README.md)

## Source-code protection model

The website's security controls belong in **server-side request handlers, access-control policies, templates, API boundaries, data-access layers, configuration and deployment pipelines**. Browser JavaScript and UI controls cannot be trusted to authorize requests.

| Code domain | Requirements | Validation examples |
|---|---|---|
| Identity | Strong credential policy, risk-appropriate MFA, protected account recovery, login throttling and enumeration resistance | Valid owner flow and refused invalid/reset flows |
| Authorization | Every object and action checked server-side; deny-by-default on admin functions; tenant separation | User A cannot retrieve/edit User B data, even by changing IDs |
| Sessions | Secure and HttpOnly cookies, appropriate SameSite, rotation on privilege changes, expiry/invalidation | Revoked and expired sessions fail; safe login/logout |
| CSRF | Defend browser-initiated state changes; use appropriate tokens/origin controls according to client model | Cross-origin state-changing request denied |
| Input validation | Typed schema, limits, safe normalization, correct file parsing | Invalid size/type and malformed inputs rejected |
| Database | Parameterised queries and constrained DB principal; avoid constructing SQL from untrusted strings | Data access bounded to intended query and role |
| Output handling | Context-sensitive escaping and templating; safe markup rendering; XSS-focused CSP where appropriate | Special characters render safely in different output contexts |
| SSRF & outbound calls | Constrain server-side fetch destinations, redirect handling and metadata/service access | Unapproved internal address and redirect paths rejected |
| File upload | Verify content type, size, extension, malware needs; store outside executable path | Uploaded content cannot execute on server |
| APIs | Verified tokens/scopes, object authorization, quotas, replay defense where appropriate | Unauthorized scope and object access rejected |
| Headers & browser policies | Tested CSP, HSTS, X-Content-Type-Options, Referrer-Policy, frame controls and Permissions-Policy as applicable | Header presence and browser functional regression |
| Secrets | Separate production secrets from source/browser bundle; protect CI and log output | Repository/secret scans and access review |
| Dependencies | Maintained versions, SBOM where required, lockfiles, checksums and security patch lifecycle | Known vulnerable dependency triage and rebuild proof |
| Errors & privacy | No stack traces or secrets to public users; minimise and redact logs | Safe error responses and redaction tests |
| Payments | Hosted checkout where suitable; verify webhooks, idempotency and authoritative order state | Forged, duplicate and out-of-order messages do not trigger fulfillment |

## Security headers: engineering cautions

- **CSP** should be defined against actual scripts/styles/frames, prefer strict nonce/hash approach where feasible, and run in Report-Only during migration before enforcement.
- **HSTS** requires consistent HTTPS across applicable hosts; consider preload only after verifying long-term operational readiness.
- **CORS** governs browser read access; it is *not* an authorization system or protection for non-browser clients.
- **SameSite** is defense in depth, not an automatic substitute for CSRF protections across all flows.
- Remove identifying server banners where feasible, but do not present metadata reduction as a fix for unpatched software.

## Secure development lifecycle

1. Identify assets, sensitive data, trust boundaries and likely attackers.
2. Define security requirements and acceptance cases before implementation.
3. Review code and dependencies; implement access checks server-side.
4. Test normal/abuse flows in staging; record denied and allowed cases.
5. Deploy through scoped and auditable release process; verify secrets and rollback.
6. Re-test after patching, feature additions, hosting migration and material provider changes.

## Source-code evidence to preserve privately

- Role/permission matrix and data-flow model.
- Automated tests for access control, session invalidation and data isolation.
- Dependency inventory and code-review results, including remediation disposition.
- Header/CSP compatibility checks and approved exceptions.
- Payment integrity tests where the system uses payment providers.
- Audit and service-recovery evidence without exposing customer data.

**Not every feature requires every control.** A static brochure website and a banking portal have different threat models; implement and test only relevant protections with documented rationale.

[Security Gates →](SECURITY_GATES.md) · [Engagement & Verification →](ENGAGEMENT_AND_VERIFICATION.md) · [OWASP ASVS →](https://asvs.owasp.org/)
