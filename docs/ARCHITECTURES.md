# ECS — Website Security Reference Architectures

**Scope:** Architectural examples, not production-as-built diagrams. **Owner:** Elite Cybersecurity Specialist (ECS). **Updated:** October 2026.

[← Back to README](../README.md)

## Architectural goals

1. Separate the visitor/customer plane from the management/operator plane.
2. Minimise public exposure while retaining the availability, performance and administrative capabilities the customer requires.
3. Harden the host, application code, deployment and account recovery as one system.
4. Keep payment-data exposure and retained personal data as low as business needs permit.
5. Verify controls under supported IPv4/IPv6, reconnect/reboot, service-restart and recovery scenarios.

## Method 1 — Secure Public Internet

![Secure public architecture](../assets/7-anonimity.png)

### Trust-boundary model

| Path | Proposed component | Boundary / validation requirement |
|---|---|---|
| Operator | Hardened laptop | Endpoint is not automatically trusted; encrypted disk, secure updates, device access controls and credential separation |
| Operator | VPN or Tor route (if selected) | No implicit trust from privacy route; confirm DNS/network behaviour and routing policy with authorized tests |
| Operator | Admin gateway / SSH | Restrict source and identities; keys, strong MFA where architecture supports it, audit logs, independent break-glass path |
| Operator | Private hosting | Least-privilege admin/service users, restricted management plane and recovery access |
| Customer | Public domain / DNS | Registrar account hardening, verified zone ownership, DNSSEC where supported, controlled record changes |
| Customer | CDN/WAF/edge (optional) | Terminate/forward TLS safely, validate proxy identity, apply rules without bypassing origin or breaking the app |
| Customer | Ingress tunnel / private route | Origin must enforce that traffic only arrives through trusted path; tunnel is not sufficient on its own |
| Customer | Hardened web service | Correct Host handling, allowed ports, safe web-server configuration, no accidental debug/admin endpoints |
| Customer | Application | Authorization, session/security headers, data minimisation, server-side validation |
| Customer | Payment provider (optional) | Provider-hosted payment where possible; trusted webhook signatures, idempotency and independent payment status checks |

### Reference traffic flows

```text
Operator → Hardened workstation → Authorized admin route → Admin authentication → Restricted VPS

Customer → DNS → [Optional edge/WAF/CDN] → [Tunnel or restricted ingress]
         → Hardened origin web server → Website/application
         → [Provider-hosted checkout where appropriate]
```

### Specific design caveats

- **Cloudflare Tunnel** in the illustration is a possible implementation, not mandatory for every customer. Any other suitable authenticated private ingress may be considered.
- DNS privacy/WHOIS redaction do not guarantee infrastructure anonymity; certificate transparency, other domains, email, metadata and application content may expose relationships.
- Origin bypass must be tested: direct IP requests, alternative hostnames, IPv4/IPv6 listeners, public management ports, old DNS records and forgotten load balancers.
- A WAF may filter known request patterns but cannot be relied upon to fix broken application authorization.
- Payment providers reduce card-handling scope only when integration is correctly designed; webhook and order-state attacks still require application controls.
- If the edge/tunnel goes down, consider maintenance and disaster-recovery impact.

### Acceptance evidence

- External and internal service-inventory snapshots, permitted/blocked traffic tests; no IPs or keys in public evidence.
- Administrator access tests (accepted/denied), least-privilege proof and privileged event logs.
- TLS configuration and origin restriction tests, including IPv6 and alternate hostname checks.
- Vulnerability remediation and regression acceptance, payment-webhook verification where present.
- Restored backup/rebuild drill and dependency/failure exercise.

## Method 2 — Tor Private Internet

![Tor-only architecture](../assets/8-tor.png)

### Trust-boundary model

| Path | Proposed component | Boundary / validation requirement |
|---|---|---|
| Operator | Hardened endpoint | Separate user and administrator activities, credentials, browser profiles and recovery access |
| Operator | Tor-aware management | Restricted management service with independent strong authentication; not simply exposed SSH on an onion URL without a threat model |
| Operator | Host / onion key custody | Secure onion-service private keys, encrypted backups, file ownership and recovery procedure |
| Visitor | Tor Browser | Browser/device security and user behaviour remain part of exposure model |
| Visitor | Tor network / onion service | Onion-service rendezvous, no conventional public-web ingress unless separately approved |
| Visitor | Hidden website | Bind services to intended interfaces, enforce server-side defenses and restrict metadata and logs appropriately |
| Visitor | Payment workflow (optional) | Payment privacy, accounting, fraud and legal requirements; blockchain transfer traces may persist |

### Reference traffic flows

```text
Operator → Hardened workstation → Tor-aware authorized management → Restricted host

Visitor (Tor Browser) → .onion address → Tor onion service → Bound private web service
                       → Minimal application → [Separate lawful payment workflow]
```

### Specific design caveats

- Onion service use reduces certain network-origin disclosures but does not prove personal anonymity or eliminate endpoint, browser, financial or application correlation risks.
- No publicly routable website listener is expected for the Tor-only public-facing application: confirm service bindings, host firewall, IPv4/IPv6, cloud security groups and fallback endpoints.
- Onion-service private keys must not enter public CI logs, Git commits, public images or unencrypted off-site storage.
- Abuse handling, application-layer rate limits and monitoring must be compatible with Tor-specific deployment realities.
- Cryptocurrency payment is an **optional business decision**, not a hardening requirement. Cryptocurrencies differ in on-chain disclosure; avoid absolute privacy claims.
- Disaster recovery includes Tor key restoration procedures, visitor communication plans and key compromise response.

### Acceptance evidence

- Authenticated Tor-only access works for approved client paths.
- Negative tests show no unapproved clearnet listener or origin endpoint.
- Restart/boot regression tests preserve intended service isolation.
- App authentication/authorization, input handling, administration and backup restore are tested.

## Shared hardening principles

- Baseline the OS/web server before changes and back up current state.
- Use applicable CIS Benchmark / DISA STIG/SRG objectives, not an incompatible OS script.
- Apply separation of duty, least privilege, mandatory access control where supported, and minimisation of writable paths.
- Validate DNS/routing in both operator and visitor contexts: *privacy path* and *customer path* have different objectives.
- Keep alerting and forensic access compatible with data minimisation policies.
- Do not publish client addresses, raw firewall rules, credentials, private keys or backup locations.

[Hosting Hardening →](HOSTING_HARDENING.md) · [Security Gates →](SECURITY_GATES.md) · [Engagement Verification →](ENGAGEMENT_AND_VERIFICATION.md)
