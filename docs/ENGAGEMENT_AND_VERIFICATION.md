# ECS — Customer Engagement and Security Verification

**Operating cycle:** HARDEN → TEST → VALIDATE → REMEDIATE → RE-TEST → MAINTAIN.

[← Back to README](../README.md)

## Engagement planning and approval

Before changing or testing a system, agree on:

- **Customer ownership and authorization:** written scope, designated authorized contacts, permitted systems and defined test window.
- **Business function and availability:** approved website features, peak hours, recovery time and recovery point objectives where defined.
- **Hosting responsibility:** exact assets operated by customer, ECS and third parties; managed services and inherited controls.
- **Security priorities:** type/sensitivity of data, privileged users, expected visitors, payment/data-processing model, attacker motivations.
- **Test rules:** whether black-box, gray-box, white-box testing is approved; exclusions, stop conditions, notification and emergency contact.
- **Success criteria:** which defenses must work, which behaviors must be denied and what disruptions cannot be tolerated.

## Engineering phases and deliverables

| Phase | Work | Private customer deliverables |
|---|---|---|
| **1. Understand** | Scope and threat model | Asset/service inventory; data-flow/trust-boundary view; responsibility matrix |
| **2. Baseline** | Observe current state safely | Baseline security inventory; prioritized findings; restore readiness |
| **3. Plan** | Select platform-compatible controls | Proposed changes, mapped sources, business impact, exceptions and rollback |
| **4. Harden** | Apply approved host/web/security-code controls | Change record and implementation evidence |
| **5. Test / validate** | Approved functional and security assessment | Positive and negative test results, severity and reproducibility |
| **6. Remediate / retest** | Correct gaps and verify corrections | Findings disposition, retest evidence, outstanding risks |
| **7. Maintain** | Monitoring, patch, incident and recovery process | Operational playbook and revalidation triggers |

## Evidence taxonomy

Use consistent terms rather than a single ambiguous "secured" label:

- **Proposed:** listed in the architecture or requirements but not applied.
- **Configured:** intended setting, version, policy or file exists.
- **Runtime verified:** positive and negative behavior tested within scope.
- **Persistence verified:** control survives relevant reboot/redeploy/reconnect.
- **Recovery verified:** clean restore or rollback tested.
- **Owner accepted:** customer accepts documented outcomes and exceptions.
- **Externally assessed/certified:** only when separately contracted and established by the relevant assessor.

## Example acceptance matrix

| Control | Positive case | Negative case | Recovery / persistence |
|---|---|---|---|
| Private administration | Approved operator can administer | Unauthorized identity/route denied | Break-glass process tested; persists after restart |
| Origin isolation | Intended edge/onion visitor flow operates | Direct public origin access blocked | Service restart/redeploy does not introduce a listener |
| Website authorization | User reads permitted objects | Cross-user/tenant object access denied | Rights changes and logout invalidate access |
| Payment webhook | Valid signed callback updates state once | Invalid signature/duplicate rejected or safely handled | Queued retries and recovery do not double-charge/fulfill |
| Firewall | Required public/app routes pass | Unapproved service exposure denied | Rules survive reboot/redeploy |
| Backup | Restored service functions normally | Recovery material not reachable through app/public host | Restore drill logged and approved |

## Controlled change protocol

1. Document rollback before high-impact host controls are installed.
2. Apply high-risk changes to a representative staging environment where possible.
3. Prioritize availability-sensitive checks (renewal, monitoring, payments, DNS, backup, deployment, emergency access).
4. Execute approved changes in a recorded window; capture relevant evidence without copying secrets into tickets.
5. Re-evaluate affected trust boundaries and re-test functionality.
6. Record every accepted exception and an owner for reviewing it.

## Customer reporting

Final customer documentation may include a sanitized architecture diagram, explicit platform scope, controls and test methodology, finding severity, corrective measures, residual risks, owners, and next review date. Never publish confidential customer-specific hardening blueprints as public repository files.

## Monitoring and incident support

- Assign alert owners and an escalation path, including contact during hosting outages.
- Document credential/token revocation and secret rotation after suspected compromise.
- Maintain host/source/dependency patch SLAs that fit the risk and business impact.
- Schedule restoration testing and evaluate full-stack failure modes, including outage of CDN/tunnel/payment providers.

**Only conduct penetration tests against systems specifically authorized by their owners.** Reference diagrams are not permission to test any third-party edge, payment, hosting or Tor infrastructure.

[Hosting Hardening →](HOSTING_HARDENING.md) · [Security Policy →](../SECURITY.md)
