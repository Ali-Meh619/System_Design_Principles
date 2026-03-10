# IAM, Secrets & Governance

> Cloud security is not only "use encryption." Real platform maturity shows up in how identities are issued, how secrets are rotated, and how blast radius is constrained when someone makes a mistake.

---

## IAM Comes First

In cloud systems, every action should be attributable to an identity:

- a human engineer
- a CI/CD pipeline
- an application service
- a batch worker
- an external customer or partner

### The core rule

Prefer **roles and short-lived credentials** over static keys stored in config files.

That single sentence already improves most interview answers.

---

## Principals, Roles, and Policies

| Concept | Meaning | Best practice |
|--------|---------|---------------|
| **Principal** | The entity making the request | Human, service account, workload, external identity |
| **Role** | A bundle of permissions assumed temporarily | Use for workloads and cross-account access |
| **Policy** | The rules controlling access | Keep narrow, explicit, and auditable |

### Least-privilege examples

- API service can read from its config bucket but cannot list every bucket
- worker can consume one queue but cannot publish admin events
- CI can deploy to staging but not production without approval

Overly broad IAM is one of the most common real-world cloud failures.

---

## Human Identity vs Workload Identity

Treat them differently.

| Identity type | Preferred pattern |
|--------------|-------------------|
| **Humans** | SSO, MFA, short-lived sessions, role assumption, audited admin access |
| **Workloads** | Instance profiles, workload identity federation, service accounts, short-lived tokens |
| **External partners** | Dedicated scoped credentials, API keys plus signing or OAuth, rotation and revocation |

If your workloads still rely on hardcoded long-lived credentials, the design is already weaker than it needs to be.

---

## Secrets and Key Management

| Secret type | Examples | Better storage |
|------------|----------|----------------|
| **Application secrets** | DB password, third-party API token | Secrets manager or vault |
| **Encryption keys** | Data key, signing key, envelope key | Managed KMS / HSM-backed service |
| **Certificates** | TLS certs, client certs | Managed certificate lifecycle where possible |

### Strong default

- secrets live in a managed secret store
- workloads fetch them at runtime using identity
- keys rotate automatically or on schedule
- access is audited

This is much stronger than environment variables baked into deployment manifests.

---

## Governance and Guardrails

Security is not just permission granularity. It is also preventing the wrong architecture from being created in the first place.

### Common governance controls

- production account restrictions
- mandatory tagging for owner, service, and cost center
- encryption-by-default policies
- audit logs for all privileged actions
- policy-as-code or organization-level controls preventing forbidden resources
- approval workflow for critical IAM or network changes

These controls reduce blast radius even when individual teams move quickly.

---

## Recommended Default

For most cloud-native interview systems:

1. Centralize human identity with **SSO + MFA**
2. Use **roles or workload identity** for services
3. Store secrets in a **managed secret store**
4. Use **KMS-backed encryption** for sensitive data and tokens
5. Enable **audit logging** for IAM, secret reads, admin actions, and key operations
6. Enforce **least privilege and environment separation** with organization/account boundaries

This gives you a strong, explainable security baseline without needing to invent a custom platform.

---

## Failure Modes

| Failure mode | What happens | Mitigation |
|-------------|--------------|------------|
| **Static credentials in code or CI** | Secret leaks, hard rotation, large blast radius | Roles, workload identity, runtime secret fetch |
| **Overly broad IAM policy** | Compromised service can access too much | Scoped roles, permission review, policy linting |
| **No audit trail** | Incident response becomes guesswork | Centralized immutable audit logs |
| **Manual key and cert lifecycle** | Expiry outages or forgotten rotation | Managed rotation and alerting |
| **Weak environment boundaries** | Staging or developer actions affect production | Separate accounts/projects, stricter prod access |

---

## Metrics

- **Number of long-lived static credentials**
- **Secrets rotation age / overdue rotations**
- **Failed and successful privilege-escalation attempts**
- **Audit log coverage**
- **Policy violations prevented by guardrails**
- **Time to revoke compromised access**

If you cannot measure these, your cloud governance is mostly aspirational.

---

## Interview Answer Sketch

"I would separate human identity from workload identity. Engineers should use SSO, MFA, and short-lived role assumption, while services should authenticate through workload roles instead of static credentials. Secrets would live in a managed secret store and encryption keys in KMS-backed services with audit logs enabled. I would also enforce least privilege, separate production from non-production, and add organization-level guardrails so teams cannot accidentally create insecure resources even under delivery pressure."
