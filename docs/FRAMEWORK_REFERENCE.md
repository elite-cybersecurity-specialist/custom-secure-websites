# ECS — Security Frameworks and Authoritative References

**Use case:** source references for platform-tailored website hosting and application hardening. No third-party audit or certification is implied by this public repository.

[← Back to README](../README.md)

| Framework / standard | Why ECS references it | Scope qualification |
|---|---|---|
| **CIS Benchmarks (Level 2 where suitable)** | Defense-in-depth configuration profiles for supported operating systems and software | Choose the exact version and server/profile; Level 2 may affect compatibility |
| **DISA STIG / SRG** | Security requirements for applicable OS, network, web, authentication, logging and operations | Apply platform-relevant requirements, not arbitrary STIG fixes on unrelated systems |
| **OWASP ASVS 5.0** | Application security requirements for coding, architecture and technical verification | Choose requirements appropriate for app and risk; verify in actual implementation |
| **OWASP API Security** | API authorization, authentication, object access and abuse prevention | Only when APIs exist or are within assessment scope |
| **NIST SP 800-53** | Security/privacy controls for access, logging, configuration, incident response and resilience | Control selection depends on customer obligations and threats |
| **NIST SP 800-207 Zero Trust** | Never rely only on assumed network trust for access decisions | An architecture principle, not a claim of product accreditation |
| **NIST SSDF (SP 800-218)** | Secure software-development and supply-chain practices | Development and CI/CD practices must be actually implemented and reviewed |
| **CISA Secure by Design** | Reduce default attack surface and prioritize security across lifecycle | Design guidance, not a vendor certification |

## Primary sources

- **CIS Benchmarks:** https://www.cisecurity.org/cis-benchmarks
- **CIS Benchmarks FAQ (Level 2):** https://www.cisecurity.org/cis-benchmarks/cis-benchmarks-faq
- **DISA public cyber standards:** https://public.cyber.mil/stigs/
- **OWASP ASVS 5.0.0:** https://asvs.owasp.org/
- **OWASP API Security:** https://owasp.org/www-project-api-security/
- **NIST SP 800-53:** https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final
- **NIST SP 800-207:** https://csrc.nist.gov/pubs/sp/800/207/final
- **NIST SP 800-218:** https://csrc.nist.gov/pubs/sp/800/218/final
- **CISA Secure by Design:** https://www.cisa.gov/securebydesign
- **ECS Kicksecure public methodology:** https://github.com/elite-cybersecurity-specialist/hardening-challenge

## How ECS should map an engagement

For each selected recommendation/requirement, record the **framework and exact published version**, asset ID, applicability, owner-approved interpretation, current state, required state, configuration evidence, runtime test, persistence/recovery test, reason for exceptions and signoff. An exact numeric compliance score requires explicit control mapping and scope; do not derive it from marketing material or a reference diagram.

## Compatibility and statements of scope

- CIS Level 2 is **not automatically safe for every production host**; its defense-in-depth controls can affect function and performance.
- DISA STIG/SRG **alignment** is not the same as a DISA security approval or certification.
- OWASP ASVS **inspiration** is not the same as a completed ASVS verification assessment.
- An OS hardening program (including ECS's KHAHP work) is methodology evidence; it is not automatic assurance for a separate web application.

[Hosting Hardening →](HOSTING_HARDENING.md) · [Website Code Security →](WEBSITE_CODE_SECURITY.md)
