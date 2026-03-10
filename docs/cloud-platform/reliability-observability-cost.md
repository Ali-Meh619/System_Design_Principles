# Reliability, Observability & Cost

> A cloud architecture is not production-ready just because it deploys. It needs clear failure tolerance, measurable health, recoverability, and cost control that still works when traffic or experiments scale up.

---

## Reliability Starts with Failure Scope

The first question is not "how available do we want to be?" but "what failures must we survive?"

| Failure scope | Typical response |
|--------------|------------------|
| **Instance failure** | Replace unhealthy node, reschedule container, retry safely |
| **AZ failure** | Run across multiple AZs with redundant app and data layers |
| **Region failure** | DR plan or multi-region architecture depending on business need |
| **Dependency failure** | Timeouts, retries, fallbacks, circuit breakers, graceful degradation |

### Good default

Most systems should start with **multi-AZ high availability in one region**. Multi-region is usually a business decision, not a default engineering reflex.

---

## Disaster Recovery: RTO and RPO

Interviewers love asking for backup strategy, but the right answer depends on recovery targets.

| Term | Meaning | Example |
|------|---------|---------|
| **RTO** | Recovery Time Objective: how long you can be down | "We must recover within 30 minutes" |
| **RPO** | Recovery Point Objective: how much data loss is acceptable | "We can lose at most 5 minutes of data" |

### Design implications

- low RTO + low RPO often pushes you toward replication and automated failover
- higher RTO/RPO may make periodic backups sufficient
- databases, object storage, and queues can all have different recovery requirements

Always make backup strategy explicit:

- snapshot schedule
- retention window
- restore testing
- cross-region copy if needed

---

## Observability Is a Product Requirement

Cloud incidents are much harder to manage if the system is opaque.

### The minimum set

- **metrics** for traffic, latency, errors, saturation
- **logs** for request context, failures, security-sensitive actions
- **traces** for multi-service latency and dependency debugging
- **alerts** tied to SLOs, not random dashboards nobody watches

### Useful service-level metrics

| Layer | Metrics |
|------|---------|
| **API tier** | QPS, p95/p99 latency, error rate, concurrency |
| **Workers** | queue lag, processing duration, retry rate |
| **Database** | CPU, storage, connection pool, replication lag, slow queries |
| **Platform** | autoscaling events, unhealthy targets, deployment failures |

---

## Cost Is a Design Constraint

Cloud cost problems are usually architecture problems in disguise.

### Common cost drivers

- overprovisioned compute
- idle development clusters
- excessive cross-zone or cross-region traffic
- high NAT / egress usage
- chatty microservices
- retaining too much hot storage
- expensive always-on GPU or inference capacity

### Cost controls worth mentioning

- right-size compute after measuring actual usage
- scale workers on queue depth instead of fixed fleet size
- use reserved/committed capacity for stable workloads
- use spot/preemptible capacity for interruptible jobs
- move cold data to cheaper storage tiers
- tag resources so cost is attributable to an owner
- create budget alarms before finance finds the issue first

---

## Recommended Default

For a serious but not overengineered production system:

1. Run the service in **one region across multiple AZs**
2. Define **SLOs** and alert on error budget burn, not only raw CPU
3. Set **backups, retention, and restore drills**
4. Use **graceful degradation** when dependencies fail
5. Add **cost dashboards and budget alarms** from the start
6. Justify multi-region with explicit **RTO/RPO, revenue impact, or compliance needs**

---

## Failure Modes

| Failure mode | What happens | Mitigation |
|-------------|--------------|------------|
| **Multi-region too early** | Huge cost and ops burden without real business value | Start multi-AZ unless requirements say otherwise |
| **No restore testing** | Backups exist on paper but fail in reality | Scheduled restore drills and runbooks |
| **Alert noise** | On-call ignores pages | SLO-based alerts, deduplication, ownership |
| **Dependency hard failure** | One service outage cascades through the stack | Timeouts, retries, circuit breakers, fallback responses |
| **Unowned spend growth** | Costs rise with no team accountable | Tagging, chargeback/showback, budget alerts |

---

## Metrics

- **Availability and SLO attainment**
- **Error budget burn**
- **Mean time to detect / mean time to recover**
- **Replication lag and backup success rate**
- **Restore test success**
- **Unit economics** such as cost per request, cost per active user, cost per job, or cost per inference

These metrics tie reliability and cloud spend back to product reality.

---

## Interview Answer Sketch

"I would begin with a single-region, multi-AZ deployment because it covers common infrastructure failures without the full cost and complexity of active-active multi-region. I would define SLOs for availability and latency, instrument the service with metrics, logs, and traces, and alert on error-budget burn rather than noisy infrastructure-only signals. For recovery, I would make RTO and RPO explicit, set backup and restore procedures, and test restores regularly. I would also add cost guardrails such as tagging, budget alerts, and right-sizing so the platform remains financially sustainable as traffic grows."
