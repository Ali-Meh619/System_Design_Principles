# Cloud Fundamentals & Shared Responsibility

> Before debating Kubernetes, Terraform, or multi-region failover, you need the basics: failure domains, managed service boundaries, and who is actually responsible for what in the cloud.

---

## The Core Mental Model

Cloud is best understood as a stack of abstractions:

| Layer | What you get | What you still own |
|------|---------------|--------------------|
| **IaaS / VMs** | Raw compute, storage, networking primitives | OS patching, runtime, deployment, scaling, app security |
| **Containers / managed orchestration** | Better packaging and scheduling | Images, app runtime, traffic policy, scaling policy |
| **Serverless / FaaS** | No server fleet management | Function code, cold start trade-offs, concurrency behavior |
| **Managed data services** | Databases, queues, object storage | Schema, access control, retention, backup policy, app-level correctness |

The interviewer is usually testing whether you can choose the right abstraction level for the problem, not whether you remember every AWS or GCP product name.

---

## Regions, Availability Zones, and Failure Domains

The most common cloud architecture mistake in interviews is speaking vaguely about "the cloud" without naming failure domains.

| Concept | Meaning | Typical use |
|--------|---------|-------------|
| **Region** | A geographic area with multiple isolated data centers | Data residency, latency, disaster boundaries |
| **Availability Zone (AZ)** | An isolated failure domain within a region | High availability without going multi-region |
| **Rack / host** | Lower-level failure domains the provider manages internally | Usually abstracted away unless discussing hardware placement |

### Interview default

- Start with **one region, multi-AZ**
- Move to **multi-region** only if you need disaster recovery, low-latency global users, or legal/data sovereignty reasons
- Say explicitly what failures you tolerate: instance, AZ, region

That framing is much stronger than saying "we deploy to the cloud for scalability."

---

## Shared Responsibility Model

Every major provider follows a version of this split:

| Area | Provider responsibility | Your responsibility |
|------|--------------------------|---------------------|
| Physical security | Data centers, hardware, power, network backbone | None directly |
| Hypervisor / managed control plane | Virtualization layer, managed service internals | Trust but verify through service selection and config |
| Guest OS / runtime | Sometimes provider, often you | Patching, vulnerable packages, hardening |
| Application | No | Correctness, auth, rate limits, encryption usage |
| Identity and data access | Identity primitives exist | Least privilege, key rotation, audit, tenant isolation |

### Short version worth saying

"The cloud provider secures the infrastructure; we still secure identities, application behavior, data access, and configuration."

---

## Managed Services vs Self-Managed Systems

| Choice | When it wins | Main risk |
|-------|---------------|----------|
| **Managed service** | Faster delivery, smaller team, standard workloads | Less control, vendor lock-in, surprise limits |
| **Self-managed on VMs/Kubernetes** | Heavy customization, unusual performance needs, portability requirements | Operational burden, patching, on-call complexity |

### Good senior default

Prefer managed services for:

- relational databases
- object storage
- queues and streaming when scale is standard
- load balancers
- secret storage and key management

Move to self-managed only when you can clearly justify the operational trade-off.

---

## Environments and Account Boundaries

Cloud sprawl becomes dangerous fast if environments are loosely separated.

### Strong baseline

- separate **prod** from **non-prod**
- use separate cloud accounts or subscriptions for stronger isolation where possible
- restrict who can touch production
- enforce naming, tagging, and budget ownership per environment

### Why this matters

Accidental production access, cost leaks, and overly broad permissions are often organizational failures before they are technical ones.

---

## Recommended Default

For most interview systems:

1. Deploy in **one region across 2-3 AZs**
2. Use **managed databases, object storage, queues, and load balancers**
3. Keep services **stateless** so they can scale horizontally
4. Split **prod** and **non-prod** with strong IAM boundaries
5. Add **multi-region DR** only if RTO/RPO or business criticality actually requires it

This gives you availability, sane operations, and room to grow without overengineering day one.

---

## Failure Modes

| Failure mode | What happens | Mitigation |
|-------------|--------------|------------|
| **Single-AZ dependency** | One zone outage takes down the service | Multi-AZ deployment, zonal load balancing, replicated data |
| **Wrong abstraction choice** | Team spends months operating infrastructure instead of shipping product | Prefer managed services first |
| **Weak environment separation** | Dev/test actions impact production | Separate accounts, IAM boundaries, guarded CI/CD |
| **Misunderstood provider responsibility** | Team assumes provider handles app-layer security or backups | Explicit ownership model and runbooks |
| **Region-wide dependency** | Entire service unavailable during region incident | DR strategy, backups, optional multi-region failover |

---

## Metrics

- **Availability by failure domain**: instance, AZ, region
- **Provisioning lead time**: how fast teams can safely deploy new services
- **Operational overhead**: number of self-managed components and on-call burden
- **Backup success and restore success**
- **Cloud resource sprawl**: unused volumes, idle instances, orphaned IPs, forgotten dev clusters

---

## Interview Answer Sketch

"I would start in a single region across multiple availability zones because that handles the most common infrastructure failure without the complexity of active-active multi-region. I would lean on managed services for storage, queues, and load balancing so the team spends time on product logic instead of operating undifferentiated infrastructure. The cloud provider handles the physical and lower-level platform security, but we still own identity, configuration, application security, backups, and tenant isolation. If the business later needs stricter disaster recovery or global latency improvement, I would extend the design to multi-region with explicit RTO and RPO targets."
