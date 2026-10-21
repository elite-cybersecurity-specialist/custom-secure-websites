# ECS — Eight Security Gates

**Architectural quality gates — proposed engineering checklist, not customer acceptance results.**

[← Back to README](../README.md)

## Assurance record for each gate

For each customer engagement, maintain a private record with **Asset/role · Threat · Requirement · Existing baseline · Proposed change · Owner approval · Configuration evidence · Positive test · Negative test · Persistence / recovery test · Compatibility result · Exception/rollback · Date and reviewer**.

A **planned** control is not an **implemented** control; an implemented control is not **verified** until the relevant tests pass. External certification requires a separate assessment.

## Gate 1 — Operator / Laptop Security

- Harden operator device and supported OS, full-disk encryption, verified boot where available.
- Separate daily-use identities from administrative identities; require phishing-resistant MFA where supported.
- Harden browser and credential storage; control removable devices and local privileges.
- Protect backups and recovery material; verify lost-device response.
- **Acceptance examples:** privilege-boundary negative test, disk-encryption evidence, update and recovery test.

## Gate 2 — Network Identity & Administrative Connection

- Identify management services requiring access and the approved route: direct private path, VPN, restricted gateway or Tor as appropriate.
- Minimise DNS leaks and route bypass, verify both IPv4 and IPv6 behaviour.
- Prevent management authentication from relying solely on source IP or obscurity.
- **Acceptance examples:** only authorized admin paths work, route fail-closed where required, rejected unauthorised session evidence.

## Gate 3 — Infrastructure Accounts Security

- Inventory domain registrar, DNS, edge provider, hosting, CI/CD, code, backup and payment identities.
- Restrict roles, enforce MFA where available, separate emergency access from daily workflow.
- Audit API tokens, repository deploy credentials, recovery channels, rotation and logging.
- **Acceptance examples:** permission matrix, token scope review, lost-factor recovery drill.

## Gate 4 — Private VPS / Hosting Security

- Select OS- and service-specific CIS Level 2 recommendations / applicable STIG/SRG security objectives.
- Restrict internet-exposed services, harden SSH, kernel, filesystem, package updates, firewall and runtime isolation.
- Implement meaningful audit events, integrity monitoring, off-host encrypted backups and tested restore.
- **Acceptance examples:** service inventory, allowed/blocked port matrix, reboot persistence and restore evidence.

## Gate 5 — Domain/DNS/Edge *or* Onion Service

**Public web:** DNS records and DNSSEC applicability; registrar controls; edge route, WAF/DDoS availability where chosen; origin bypass testing.

**Tor-only:** onion-service keys and bindings; no unapproved clearnet exposure; Tor Browser accessibility and restoration.

- **Acceptance examples:** DNS and account change controls, origin negative tests; onion-only exposure tests if relevant.

## Gate 6 — Deployment / Supply-Chain Security

- Peer-approved changes, locked builds/dependencies where suitable, artifact verification, CI/CD credential scopes.
- Separate test/staging/production secrets, monitor high-severity dependency risk, scan for secrets before publishing.
- Implement versioned rollback and verified release provenance for customer production deployments.
- **Acceptance examples:** clean build, signed/verified artifact where adopted, rollback success, no secrets in output.

## Gate 7 — Website / Application Security

- Threat-model interfaces, authentication, authorization, sessions, CSRF, APIs and uploads.
- Enforce per-object and per-function authorization server-side; never rely only on frontend JavaScript.
- Use parameterised queries, safe serialization, contextual encoding, modern headers and safe error handling.
- **Acceptance examples:** authorized/denied user test cases, session replay controls where relevant, injection and access-boundary verification.

## Gate 8 — Payment Gateway / Wallet / Processor

- Prefer hosted checkout and minimise exposure of cardholder data where suited to business flow.
- Verify payment notifications using the provider's cryptographic or signed mechanism; prevent replay and duplicate fulfillment.
- Split payment administration permissions, log events without storing unnecessary sensitive data.
- For privacy-sensitive alternatives, verify legal/compliance and privacy implications, never assert that a blockchain payment is automatically anonymous.
- **Acceptance examples:** forged webhook rejected, duplicate webhook safely idempotent, refund/cancellation access audited.

## Change control

1. **Scope & baseline** → 2. **Proposed hardening** → 3. **Compatibility / rollback** → 4. **Authorized implementation** → 5. **Positive and negative verification** → 6. **Reboot/restore** → 7. **Owner acceptance**.

Examples intentionally omit host-specific exploit details, addresses and production configuration.

[Architecture →](ARCHITECTURES.md) · [Website Code Security →](WEBSITE_CODE_SECURITY.md)
