# ☁️ Cloud & Platform

> Cloud interview questions are usually not about memorizing vendor names. They are about showing that you understand failure domains, deployment models, IAM, networking, and cost/reliability trade-offs well enough to ship sane production defaults.

---

## Topics in This Category

| # | Topic | Difficulty | What You'll Learn |
|---|-------|-----------|------------------|
| 1 | [Cloud Fundamentals & Shared Responsibility](cloud-fundamentals.md) | 🟢 Beginner | Regions, AZs, managed services, shared responsibility, environment boundaries |
| 2 | [Compute & Deployment Patterns](compute-deployment-patterns.md) | 🟡 Mid | VMs vs containers vs Kubernetes vs serverless, autoscaling, rollout strategies |
| 3 | [Cloud Networking & Traffic Management](cloud-networking-traffic-management.md) | 🟡 Mid | VPCs, subnets, DNS, load balancers, API gateways, service-to-service traffic |
| 4 | [IAM, Secrets & Governance](iam-secrets-governance.md) | 🟡 Mid | Least privilege, workload identity, secret rotation, KMS, audit and policy controls |
| 5 | [Reliability, Observability & Cost](reliability-observability-cost.md) | 🔴 Advanced | Multi-AZ vs multi-region, DR, SLOs, cost guardrails, autoscaling economics |

---

## Why This Category Matters

System design answers often sound good until the interviewer asks:

- Where exactly does this run?
- What fails when one zone goes down?
- Why are you using containers instead of serverless here?
- How do services authenticate to each other?
- How do you stop cloud cost from exploding after launch?

Those are cloud and platform questions. Strong answers here make your design sound deployable, operable, and realistic.

---

## What To Study First

```text
General backend interviews:
  Cloud Fundamentals -> Compute & Deployment -> Cloud Networking

Platform / infra / SRE interviews:
  Cloud Networking -> Reliability, Observability & Cost -> IAM, Secrets & Governance

Senior system design interviews:
  Compute & Deployment -> IAM, Secrets & Governance -> Reliability, Observability & Cost
```

---

## Cloud Quick Map

| Interview question | Best doc to start with |
|--------------------|------------------------|
| "Single region or multi-region?" | [Reliability, Observability & Cost](reliability-observability-cost.md) |
| "Kubernetes or serverless?" | [Compute & Deployment Patterns](compute-deployment-patterns.md) |
| "What belongs in public vs private subnets?" | [Cloud Networking & Traffic Management](cloud-networking-traffic-management.md) |
| "How do workloads access databases safely?" | [IAM, Secrets & Governance](iam-secrets-governance.md) |
| "What does the cloud provider secure vs what do we secure?" | [Cloud Fundamentals & Shared Responsibility](cloud-fundamentals.md) |

---

## Defaults Worth Saying Out Loud

- Start with **one region, multi-AZ**, then justify multi-region with explicit business requirements.
- Prefer **managed services** unless you have a strong reason to own operations yourself.
- Keep application tiers **stateless** and move state to managed data systems.
- Put internet-facing entry points behind **DNS + CDN/WAF + load balancer/API gateway**.
- Use **roles and short-lived credentials**, not long-lived secrets on disk.
- Define **SLOs, budgets, and cost alarms** before traffic spikes force you to.

These defaults make your answer sound practical even before the deep dive starts.

---

## Practice Questions

1. Design a cloud deployment strategy for a B2B SaaS app that must survive one availability zone failure.
2. You are moving a monolith to the cloud. What stays on VMs, what goes to containers, and what might be serverless?
3. Design network boundaries for a public API, internal services, workers, and databases.
4. Your engineering bill doubled after a traffic spike. Walk through the cost investigation and containment plan.
5. A regulated customer asks for stricter IAM, key management, and auditability. What changes?
