# 🩵 Specialized Systems

> Complex, domain-specific systems that have entirely different requirements and trade-offs compared to standard web architectures.

---

## Topics in This Category

| # | Topic | Difficulty | What You'll Learn |
|---|-------|-----------|------------------|
| 1 | [Real-time Collaboration (Google Docs)](collaboration-editing.md) | 🔴 Advanced | Conflict-free Replicated Data Types (CRDTs), Operational Transformation (OT), WebSockets |
| 2 | [Webhooks System Design](webhooks.md) | 🔴 Advanced | Event-driven architecture, retries, exponential backoff, dead letter queues, security signatures |

---

## The Two Paradigms of Real-time Collaboration

When building systems like Google Docs, Figma, or Notion, you must choose how to merge conflicting edits from multiple concurrent users:

### 1. Operational Transformation (OT)
- **Used by**: Google Docs
- **How it works**: A central server acts as the source of truth, transforming incoming operations against the current state.
- **Pros**: Established, battle-tested for text editing.
- **Cons**: Extremely complex to implement mathematically; requires a central server.

### 2. Conflict-free Replicated Data Types (CRDTs)
- **Used by**: Figma, Notion, Apple Notes
- **How it works**: Data structures that can be updated independently and concurrently without coordination, and always converge to the same state.
- **Pros**: Truly decentralized, supports offline editing out-of-the-box.
- **Cons**: High memory overhead (tombstones for deleted text), garbage collection complexity.

---

## Anatomy of a Robust Webhooks System

Webhooks seem simple (just send an HTTP POST), but at scale they require a robust distributed system:

```
[Event Source] 
     ↓
[Event Queue (Kafka)] 
     ↓
[Dispatcher Service]  ←→ [Metadata DB (Endpoint URLs)]
     ↓
[Rate Limiter]        ←→ [Redis]
     ↓
[Worker Nodes]        →  (HTTP POST with Signature) → [Client Server]
     ↓ (failure)
[Retry Queue]         →  (Exponential Backoff)
     ↓ (max retries exceeded)
[Dead Letter Queue]
```

### Key Requirements
1. **Security**: HMAC signatures so clients can verify the payload came from you.
2. **Reliability**: Exponential backoff for retries to avoid overwhelming struggling clients.
3. **Idempotency**: Providing unique Event IDs so clients don't process the same event twice.
4. **Ordering**: Strict ordering is hard and often not guaranteed; prefer monotonic event timestamps.
