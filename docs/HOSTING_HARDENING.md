# ECS — High-Assurance Website Hosting Hardening

**Foundation 1: secure the hosting operating system and supporting services.** This document is a scoping/engineering baseline, not an executable remediation script or proof of compliance.

[← Back to README](../README.md)

## Select the actual platform before selecting controls

| Hosting model | Hardening control available to customer | Important boundary |
|---|---|---|
| Dedicated VPS / bare metal | OS, kernel, services, SSH, firewall, application confinement, web stack, filesystems | Provider hypervisor/control plane still outside local ownership |
| Managed VM / hosting service | Varies: OS and networking may be shared with provider responsibility | Never claim control over layers managed by provider |
| Containers / orchestrated platform | Container runtime, images, namespaces, network policies, pod/service identity, host where owned | Container isolation alone is not equivalent to hardened host |
| PaaS / serverless | IAM, application/deployment, edge, secrets, logging, runtime settings | Underlying OS is provider-managed |
| Shared web hosting | Application settings, provider control panel, access, supported file permissions | Typically cannot implement kernel/SSH/firewall baselines directly |

## Reference standards and correct interpretation

- **CIS Benchmarks Level 2**: defense-in-depth recommendations, selected from the **correct benchmark version and platform/server profile**. Record rationale and compatibility dispositions, including any Level 1 prerequisites.
- **DISA STIG / SRG**: adapt **platform-applicable** security requirements and implementation objectives; do not apply unrelated Ubuntu/Debian/RHEL remediation automatically.
- **OWASP and NIST**: supplement web-application, identity, secure-development, logging and resilience requirements.
- Formal CIS/DISA compliance claims require an agreed audit scope, exact version mapping and the relevant external assessment process.

## Minimum engineering phases

### A. Discovery, recovery readiness and baseline

- Identify OS/kernel version and support status, owner/provider responsibility, installed packages and running services.
- Record exposure (TCP/UDP, IPv4/IPv6, public/private interfaces, security groups, DNS), management path and reverse proxy flow.
- Inventory service identities, root/admin sudo access, privileged jobs, SSH/public keys and authentication methods.
- Document website stack, runtime, database, queue, logging, cache, monitoring, certificates, cron/systemd units.
- Take valid baseline backups/snapshots and verify a rollback/recovery path before high-impact changes.

### B. Host OS / kernel / privilege protection

- Supported security updates with maintenance plan, signed package provenance and controlled reboots.
- Remove or disable only *unneeded* services; justify anything public-facing.
- Minimise privileged service accounts; per-service identities, sudo restrictions and secure secret storage.
- Review sysctl/kernel security settings against platform requirements; avoid changes that break container runtimes, monitoring or updates.
- AppArmor/SELinux profiles or equivalent where appropriate; enforce policy only after compatibility testing.
- Review mounts, ownership, read/write separation, temporary directories and filesystem integrity controls.

### C. Remote management and network boundaries

- Harden SSH authentication, user/service allowlists, administrative port exposure and forwarding features according to need.
- Enforce public inbound allowlist; use private control plane for management if possible.
- Review **egress** rules for exfiltration resistance while preserving necessary update, DNS, payment/API and monitoring traffic.
- Validate IPv6 separately; ensure cloud security groups and host firewall express consistent intent.
- Where proxied, ensure origin cannot be accessed through unintended listeners, bypass IPs or forgotten hosts.

### D. Web-server and application runtime

- Maintain supported nginx/Apache/Caddy or other runtime, modules and language versions.
- Restrict unnecessary methods, directory listing, debug pages, default vhosts and sensitive files.
- Validate Host headers, upstream client identity, proxy trust chain, forwarding headers and TLS termination.
- Keep uploads/logs/cache and application runtime files separated; disallow execution within untrusted storage.
- Run application services as constrained identities with minimal directory, socket and system access.
- Tune request-body sizes/timeouts and resource quotas for the actual workload; test availability and abuse handling.

### E. Monitoring, integrity, backups and recovery

- Centralise actionable logs and review retention versus privacy and legal requirements.
- Watch authentication changes, privilege escalation, policy changes, executable/source integrity and critical service health.
- Protect backup keys and storage outside the main host/account blast radius; periodically restore to isolated testing.
- Establish alert ownership, contact escalation and an incident response/credential rotation runbook.

## Verify, do not assume

Each significant control should have:

| Assurance stage | Example |
|---|---|
| Baseline | Record prior permitted management path and running service inventory |
| Configuration | Record configuration source and who approved a change |
| Runtime | Positive test for approved route; negative test for denied route |
| Persistence | Confirm after service restart/reboot (or redeploy for managed services) |
| Compatibility | Confirm website, API, TLS renewal, backups, monitoring and payment remain operational |
| Recovery | Demonstrate rollback or restoration in a controlled environment |

**Cautions:** A blanket `noexec`, arbitrary `/tmp` restrictions, indiscriminate sysctl changes or changing mandatory-access-control modes can impair production services. Review against the **real web stack** and document exceptions instead of claiming that maximum restriction equals maximum security.

## Kicksecure method and hosting distinction

The public [KHAHP Kicksecure hardening challenge](https://github.com/elite-cybersecurity-specialist/hardening-challenge) shows a high-assurance verification style. Controls validated for Kicksecure **do not automatically transfer** to other Linux distributions, a web-facing VPS, cloud PaaS or a managed host.

[Framework Reference →](FRAMEWORK_REFERENCE.md) · [Engagement & Verification →](ENGAGEMENT_AND_VERIFICATION.md)
