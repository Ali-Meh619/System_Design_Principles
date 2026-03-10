# Compute & Deployment Patterns

> One of the most common platform interview questions is not "can this scale?" but "what should this actually run on?" VMs, containers, Kubernetes, and serverless all solve different problems.

---

## Picking the Compute Model

| Model | Best for | Strengths | Weaknesses |
|------|----------|-----------|------------|
| **VMs** | Legacy apps, stateful software, custom OS/runtime control | Full control, predictable isolation | More ops overhead, slower deployments |
| **Containers** | Most stateless services and workers | Fast deploys, good density, consistent packaging | Need an orchestrator and image hygiene |
| **Kubernetes** | Many microservices, platform standardization, mixed workloads | Scheduling, service discovery, autoscaling, rollouts | Control-plane complexity, steep learning curve |
| **Serverless** | Event-driven jobs, spiky traffic, lightweight APIs, glue logic | No server fleet management, scale-to-zero | Cold starts, runtime limits, harder local debugging |

### Simple decision rule

- one or two services, small team: containers or serverless
- many services, shared platform team: Kubernetes becomes more attractive
- unusual system dependencies or legacy stack: VMs may still be the right answer

---

## Stateless Services Win by Default

Your app tier should usually be stateless:

- session state goes to Redis or a database
- uploaded files go to object storage
- background work goes to queues
- logs and metrics go to external systems

Why? Because stateless services can:

- autoscale horizontally
- roll forward or back cleanly
- survive instance replacement
- support blue-green or canary deploys much more easily

---

## Autoscaling Signals

CPU is useful, but senior answers usually combine multiple signals.

| Workload | Good autoscaling signals |
|---------|--------------------------|
| **Web/API tier** | CPU, memory, request concurrency, p95 latency, queue depth |
| **Workers** | Queue lag, jobs in flight, processing latency |
| **Streaming consumers** | Partition lag, event backlog, CPU |
| **Serverless** | Concurrency, execution duration, downstream saturation |

### Important nuance

Autoscaling the app tier does not help if the real bottleneck is:

- a saturated database
- exhausted downstream API quotas
- a hot partition in the queue
- a shared cache cluster that is already maxed out

Always say what the scaling target is and what dependency might become the next bottleneck.

---

## Deployment Patterns

| Pattern | How it works | Best for | Main risk |
|--------|---------------|----------|-----------|
| **Rolling** | Replace instances gradually | Standard internal services | Bad version can still spread before detection |
| **Blue-Green** | Stand up full new environment, then switch traffic | Critical releases, easy rollback | Double capacity during rollout |
| **Canary** | Send a small share of traffic to new version first | High-risk changes, model/API migrations | Needs strong metrics and fast abort logic |
| **Shadow** | Mirror live traffic without affecting user response | Safe testing of new system behavior | Extra cost, tricky result comparison |

### Strong interview default

For user-facing production services:

- stateless containers
- rolling deploys for low-risk changes
- canary for risky releases
- feature flags for behavior changes
- automatic rollback on error-rate or latency regression

---

## Batch, Async, and Scheduled Work

Not everything should run inside the request path.

### Typical split

- **Synchronous API tier**: fast user requests, low-latency reads/writes
- **Async workers**: image processing, retries, notifications, enrichment
- **Scheduled jobs**: backfills, cleanup, compaction, report generation

This separation is a core deployment decision because it changes scaling policy, failure handling, and cost.

---

## Recommended Default

For most modern interview systems:

1. Package services as **containers**
2. Run them on a managed container platform or Kubernetes if you have many services
3. Keep request-serving services **stateless**
4. Push slow work to **queues + workers**
5. Use **canary or rolling deploys** with health checks and rollback automation

If the workload is highly spiky and lightweight, explicitly consider serverless instead.

---

## Failure Modes

| Failure mode | What happens | Mitigation |
|-------------|--------------|------------|
| **Stateful app tier** | Scaling and rollbacks become painful | Externalize sessions, uploads, and jobs |
| **CPU-only autoscaling** | Latency stays bad while concurrency or queues explode | Scale on latency, backlog, or concurrency too |
| **Bad rollout spreads globally** | Wide user impact | Canary, health gates, feature flags, rollback |
| **Serverless thrash** | Cold starts and concurrency spikes hurt latency | Provisioned concurrency, warm paths, move hot services to containers |
| **Kubernetes overkill** | Team spends too much time on platform maintenance | Use managed platforms or simpler container hosting when scope is small |

---

## Metrics

- **Deployment frequency**
- **Change failure rate**
- **Mean time to rollback**
- **Startup time / cold start latency**
- **Queue lag for async workers**
- **CPU, memory, concurrency, and p95/p99 latency**

These metrics tell you whether the platform is helping the team move faster or quietly slowing everything down.

---

## Interview Answer Sketch

"I would keep the application tier stateless and deploy it as containers so instances can scale horizontally and roll forward or back safely. For compute choice, I would prefer managed containers for steady web traffic and workers, and only use serverless for event-driven or bursty jobs where scale-to-zero matters. Slow tasks such as image processing or notification fan-out should go through a queue and worker tier. For deployments, I would start with rolling updates and use canary releases plus automatic rollback on error-rate and latency regressions for higher-risk changes."
