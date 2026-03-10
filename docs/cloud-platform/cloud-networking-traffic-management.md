# Cloud Networking & Traffic Management

> Many system designs break down at the network layer: public endpoints are too exposed, internal services are too reachable, or traffic management is treated like magic. This topic turns "we put a load balancer in front" into a real answer.

---

## Core Network Building Blocks

| Building block | Purpose | Typical decision |
|---------------|---------|------------------|
| **VPC / VNet** | Private network boundary in the cloud | One per environment or account, with clear routing and security rules |
| **Public subnet** | Resources that must accept inbound internet traffic | Load balancers, bastions if still needed, managed gateways |
| **Private subnet** | Resources that should not be internet-reachable | App servers, workers, databases |
| **Internet gateway / equivalent** | Inbound and outbound internet routing | Attach to public-facing network tier |
| **NAT / egress gateway** | Outbound internet access from private workloads | Use sparingly, watch cost and bottlenecks |

### Senior default

Put only the edge tier in public subnets. Keep application and data tiers private unless there is a very explicit reason not to.

---

## North-South vs East-West Traffic

| Traffic type | Meaning | Common controls |
|-------------|---------|-----------------|
| **North-South** | User or partner traffic entering/leaving the system | DNS, CDN, WAF, API gateway, load balancer, rate limiting |
| **East-West** | Service-to-service internal traffic | Service discovery, mTLS where needed, internal load balancing, network policies |

If you say both terms clearly in an interview, it signals that you understand real platform boundaries.

---

## DNS, CDN, Load Balancer, API Gateway

These are related, but not interchangeable.

| Layer | Job |
|------|-----|
| **DNS** | Resolves names and can steer traffic across regions or environments |
| **CDN** | Caches static and cacheable content close to users |
| **WAF** | Blocks obvious malicious traffic patterns at the edge |
| **Load balancer** | Distributes requests across healthy targets |
| **API gateway** | Handles auth, quotas, request routing, transformations, and developer-facing policies |

### Common production path

```text
Client
  -> DNS
  -> CDN / WAF
  -> Load Balancer or API Gateway
  -> App services
  -> Internal services / databases
```

---

## Internal Service Communication

As systems grow, app-to-app traffic becomes as important as user traffic.

### Common needs

- service discovery so services do not hardcode IPs
- retries with backoff for transient failures
- timeouts on every network call
- clear separation between synchronous request paths and async event paths
- internal auth between services, often via mTLS or signed identity tokens

This is where many "microservices" answers become weak if networking is hand-waved.

---

## Traffic Shaping and Routing

Platform teams often need more than plain round-robin load balancing.

| Pattern | Why it exists |
|--------|----------------|
| **Weighted routing** | Canary releases, gradual migration, A/B testing |
| **Path-based routing** | `/api` to app tier, `/media` to storage/CDN-backed service |
| **Geo routing** | Send users to nearest region or data-residency-compliant region |
| **Internal-only services** | Prevent accidental public exposure |

---

## Recommended Default

For a typical cloud-native product:

1. Create a VPC/VNet per environment with public and private subnets
2. Put **CDN/WAF + load balancer or API gateway** at the edge
3. Keep services and databases in **private subnets**
4. Use **service discovery** and internal load balancing for east-west traffic
5. Apply **timeouts, retries, and rate limits** consistently at boundaries

This is simple, safe, and maps cleanly to most interview systems.

---

## Failure Modes

| Failure mode | What happens | Mitigation |
|-------------|--------------|------------|
| **Too many public resources** | Larger attack surface and harder governance | Keep only edge components public |
| **Flat network trust** | Any compromised service can laterally reach too much | Private subnets, segmentation, security groups, network policies |
| **NAT bottleneck or cost blow-up** | Outbound traffic becomes slow or expensive | Minimize unnecessary egress, use private endpoints where possible |
| **No service discovery strategy** | Manual endpoint management and brittle deploys | Internal DNS / service registry |
| **Unbounded retries** | Retry storms amplify outages | Timeouts, jitter, circuit breakers, budgets |

---

## Metrics

- **Ingress request rate and error rate**
- **p95/p99 latency by hop**
- **Load balancer healthy target count**
- **Cross-zone / cross-region traffic volume**
- **Egress spend**
- **WAF block rate and API gateway throttle rate**

These metrics expose both performance problems and architecture mistakes.

---

## Interview Answer Sketch

"I would separate the network into public and private tiers. Internet traffic would enter through DNS, then a CDN and WAF, then an API gateway or load balancer. Application services and databases would stay in private subnets, and service-to-service traffic would use internal discovery plus strict timeouts and retries. That gives us a smaller attack surface, cleaner routing, and better control over canary traffic, rate limits, and east-west communication as the system grows."
