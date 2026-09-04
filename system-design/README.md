# System Design Notes

> Essential concepts every developer should know — from junior to senior.

## Table of Contents
1. [Scalability](#scalability)
2. [Load Balancers](#load-balancers)
3. [Caching](#caching)
4. [Databases](#databases)
5. [API Patterns](#api-patterns)
6. [Message Queues](#message-queues)
7. [Microservices vs Monolith](#microservices-vs-monolith)
8. [CDN](#cdn)
9. [CAP Theorem](#cap-theorem)
10. [Rate Limiting](#rate-limiting)
11. [Security](#security)
12. [Interview Template](#system-design-interview-template)

---

## Scalability

```
Vertical Scaling (Scale Up)
→ Add more power to ONE server: CPU, RAM, faster SSD
→ Simple, no code changes needed
→ Hard limit (biggest machine available)
→ Single point of failure
→ Best for: Databases, legacy monoliths

Horizontal Scaling (Scale Out)
→ Add more servers, distribute the load
→ Needs load balancer + stateless app design
→ Unlimited theoretical capacity
→ Fault tolerant (one server dies, others handle it)
→ Best for: Web servers, microservices, APIs
```

---

## Load Balancers

```
What:  Distribute incoming traffic across multiple servers
Why:   Prevent overload, high availability, zero-downtime deploys

Algorithms:
  Round Robin        → Server 1 → 2 → 3 → 1 → 2 → 3...
  Least Connections  → Send to server with fewest active connections
  IP Hash            → Same user IP → same server (session sticky)
  Weighted           → More powerful server gets more traffic
  Random             → Random server selection

Layer 4 vs Layer 7:
  L4 (Transport) → Routes by IP/TCP — fast, less flexible
  L7 (Application) → Routes by URL, headers, cookies — more features

Tools:  Nginx, HAProxy, AWS ALB/NLB, Cloudflare, Traefik
```

---

## Caching

```
What:  Store frequently accessed data in fast memory (RAM)
Why:   Reduce DB load, speed up responses (ms vs seconds)

Cache Hierarchy:
1. L1/L2/L3 CPU Cache    → Nanoseconds
2. RAM (in-process)      → ~100ns (e.g. HashMap, dict)
3. Redis/Memcached       → ~1ms (distributed, shared)
4. CDN Edge Cache        → ~10ms (geographically close)
5. Database              → ~10-100ms
6. Disk/S3               → ~100ms-1s

Caching Strategies:
  Cache Aside (Lazy)   → App checks cache → miss → load DB → store in cache
                          Most common, works well with read-heavy apps

  Write Through        → Write to cache AND DB simultaneously
                          Consistent, slight write overhead

  Write Back/Behind    → Write to cache only, async sync to DB
                          Fast writes, risk of data loss on crash

  Read Through         → Cache handles DB reads automatically
                          Simplifies app code

Cache Invalidation (hardest problem in CS):
  TTL (Time To Live)  → Expire after N seconds
  Event-based         → Invalidate when data changes (pub/sub)
  Cache versioning    → Change key when data changes

Redis Use Cases:
  Sessions, rate limiting, leaderboards, pub/sub,
  job queues, distributed locks, real-time analytics

What NOT to cache:
  → User-specific private data (unless per-user key)
  → Highly volatile data (stock prices, live scores)
  → Infrequently accessed data
```

---

## Databases

```
SQL (Relational):
  → Structured schema, ACID transactions
  → ACID: Atomicity, Consistency, Isolation, Durability
  → Best for: Banking, ERP, e-commerce orders, anything with relations
  → Examples: PostgreSQL ★, MySQL, SQL Server, Oracle

NoSQL Types:
  Document     → MongoDB, Firestore (JSON-like docs, flexible schema)
  Key-Value    → Redis, DynamoDB (fastest, simple lookup)
  Column       → Cassandra, HBase (time-series, IoT, analytics)
  Graph        → Neo4j, Amazon Neptune (social networks, recommendations)
  Search       → Elasticsearch, OpenSearch (full-text search)

When SQL:
  → Complex relationships between data
  → Transactions matter (money, inventory)
  → Data structure is well-defined
  → Reporting / analytics queries

When NoSQL:
  → Flexible or rapidly changing schema
  → Massive scale / high throughput
  → Simple lookups (key-value)
  → Document store (user profiles, product catalogs)

Indexing:
  → Speeds up reads, slows writes, uses disk space
  → Index columns used in WHERE, JOIN, ORDER BY
  → Composite index: (user_id, created_at)
  → EXPLAIN query to check index usage
  → Don't over-index — check with real queries

Normalization vs Denormalization:
  Normalization    → Eliminate redundancy, separate tables, joins needed
                     1NF → 2NF → 3NF → BCNF
  Denormalization  → Add redundancy for read speed, fewer joins
                     Used in data warehouses, analytics, heavily-read tables

Sharding:
  → Split database horizontally across multiple servers
  → By user_id range, geography, or hash
  → Increases complexity but enables massive scale

Replication:
  → Copy data to multiple servers
  → Master-Replica: Master handles writes, replicas handle reads
  → Increases read throughput + fault tolerance
```

---

## API Patterns

```
REST:
  → Stateless, HTTP-based, resource-oriented
  → Verbs: GET POST PUT PATCH DELETE
  → Best for: Public APIs, CRUD, mobile backends

GraphQL:
  → Client asks for exactly the data it needs
  → Single /graphql endpoint
  → No over/under-fetching
  → Best for: Complex UIs, multiple clients with different needs
  → Examples: GitHub v4 API, Shopify, Facebook

gRPC:
  → Protocol Buffers (binary encoding, very fast)
  → Strongly typed schema (.proto files)
  → Supports streaming (server-side, client-side, bidirectional)
  → Best for: Microservice-to-microservice, low latency
  → HTTP/2 based

WebSocket:
  → Persistent TCP connection, full-duplex
  → Server can push to client without polling
  → Best for: Chat, live notifications, real-time dashboards, gaming

Webhook:
  → Server sends HTTP POST to your URL when an event happens
  → You don't poll — they push to you
  → Best for: Payment callbacks (Stripe), CI/CD triggers, Slack events
  → Always validate webhook signatures!

SSE (Server-Sent Events):
  → One-way: Server → Client over HTTP
  → Simpler than WebSocket for push
  → Best for: Live feeds, notifications, progress updates
```

---

## Message Queues

```
What:  Async, decoupled communication between services
Why:   Handle traffic spikes, retry on failure, decouple services

Flow:
  Producer → [Queue/Topic] → Consumer(s)

Key Concepts:
  Queue     → One producer, one consumer (point-to-point)
  Topic     → One producer, many consumers (pub/sub)
  Dead Letter Queue (DLQ) → Failed messages go here for inspection
  Acknowledgment → Consumer confirms message processed
  Retry + backoff → Retry failed messages with delay

Tools:
  RabbitMQ      → Traditional AMQP broker, flexible routing
  Apache Kafka  → High-throughput, ordered, replayable event log
  AWS SQS       → Managed queue, no infra
  AWS SNS       → Managed pub/sub
  Redis Pub/Sub → Lightweight, in-memory, not persistent
  BullMQ        → Redis-backed queue for Node.js

Use Cases:
  → Email / SMS / push notifications
  → Image / video processing
  → Order processing pipelines
  → Audit / event logging
  → Microservice communication
  → Scheduled / background jobs
```

---

## Microservices vs Monolith

```
Monolith:
  ✅ Simple to develop, test, deploy initially
  ✅ No network latency between modules
  ✅ Easier debugging (one process, one log)
  ✅ Simple transactions (one DB)
  ❌ Hard to scale individual parts
  ❌ One bug can crash everything
  ❌ Deploy everything to change anything
  ❌ Tech stack locked

Microservices:
  ✅ Independent scaling per service
  ✅ Independent deployment (deploy auth without touching orders)
  ✅ Technology freedom per service
  ✅ Fault isolation — one service down ≠ all down
  ✅ Smaller codebase per team
  ❌ Complex infra (service discovery, load balancers)
  ❌ Network latency between services
  ❌ Distributed system challenges (consistency, tracing)
  ❌ Testing harder (need to mock other services)
  ❌ Overkill for small teams

Practical Advice:
  → Start with a Monolith
  → Split into services when you have:
      • Clear bounded domains
      • Scaling bottleneck on one part
      • Multiple teams needing independent deployments
      • Different tech requirements per domain
```

---

## CDN

```
What:  Distributed servers caching static/dynamic content globally
Why:   Serve users from nearest location → lower latency

What to CDN:
  → Static: images, CSS, JS, fonts, videos
  → Dynamic: some CDNs can cache API responses with rules

How it works:
  1. User requests asset
  2. DNS routes to nearest edge PoP (Point of Presence)
  3. Cache HIT → serve immediately
  4. Cache MISS → fetch from origin → cache → serve

Cache-Control headers:
  Cache-Control: public, max-age=31536000, immutable   (1 year for hashed assets)
  Cache-Control: no-cache                              (always revalidate)
  Cache-Control: no-store                              (never cache)
  ETag: "abc123"                                       (version fingerprint)

Providers:
  Cloudflare (free tier available), AWS CloudFront,
  Fastly, Akamai, BunnyCDN, KeyCDN
```

---

## CAP Theorem

```
Distributed systems can ONLY guarantee 2 of these 3:

C — Consistency
    All nodes return the same, most recent data
A — Availability
    Every request gets a response (not guaranteed latest data)
P — Partition Tolerance
    System keeps working despite network failures between nodes

P is almost always required (networks DO fail)
→ Real choice is: C or A when partitions happen

Real-world picks:
  CP (Consistency + Partition):
    MongoDB, HBase, Zookeeper
    → Banking transactions, inventory (accuracy > availability)

  AP (Availability + Partition):
    Cassandra, DynamoDB, CouchDB
    → Social feeds, DNS, shopping carts (availability > perfect accuracy)

Note: In practice, systems exist on a spectrum and offer
      tunable consistency (eventual vs strong).
```

---

## Rate Limiting

```
Why:  Prevent abuse, DoS protection, fair usage, cost control

Algorithms:
  Fixed Window    → Count requests per time window (e.g. 100/min)
                    Simple but allows burst at window boundary

  Sliding Window  → Rolling window, smoother, more accurate
                    Slightly more memory/computation

  Token Bucket    → Tokens added at fixed rate; each request consumes one
                    Allows bursting up to bucket size
                    Most common algorithm (AWS, Stripe)

  Leaky Bucket    → Queue requests, process at fixed rate
                    Smooths traffic, excess dropped or queued

Implementation:
  → Nginx: limit_req_zone
  → Redis + middleware (most flexible)
  → API Gateway (AWS API Gateway, Kong, Traefik)
  → Cloudflare Rate Limiting

HTTP Response:
  429 Too Many Requests
  Retry-After: 60
  X-RateLimit-Limit: 100
  X-RateLimit-Remaining: 0
  X-RateLimit-Reset: 1725436800

Common Limits:
  → Per IP
  → Per user/token (authenticated)
  → Per endpoint (login stricter than GET /posts)
  → Per API key (for API products)
```

---

## Security

```
Authentication vs Authorization:
  Authentication → Who are you? (login, verify identity)
  Authorization  → What can you do? (permissions, roles)

Common Attacks & Defenses:
  SQL Injection  → Parameterized queries, ORM (never concat user input)
  XSS            → Escape output, Content-Security-Policy header
  CSRF           → CSRF tokens, SameSite cookies
  IDOR           → Check ownership on every resource request
  SSRF           → Whitelist allowed URLs/IPs for server requests
  Path Traversal → Validate/sanitize file paths
  Brute Force    → Rate limit, lockout, CAPTCHA, 2FA
  MITM           → HTTPS everywhere, HSTS header
  Clickjacking   → X-Frame-Options: DENY

Password Security:
  → Never store plaintext
  → Hash: bcrypt (cost 10-12) or Argon2id
  → Salted automatically by bcrypt/argon2
  → Check against HaveIBeenPwned on registration

JWT:
  Structure:  Header.Payload.Signature (base64url encoded)
  Store in:   HttpOnly, Secure, SameSite=Strict cookie
  NOT in:     localStorage (XSS vulnerable)
  Expiry:     Short (15min access) + Refresh token (7 days)
  Validate:   Signature, expiry, issuer, audience

HTTPS / TLS:
  → Encrypt data in transit
  → Prevents MITM and eavesdropping
  → Use HSTS header: Strict-Transport-Security: max-age=31536000
  → TLS 1.2 minimum, TLS 1.3 preferred

Security Headers:
  Content-Security-Policy: default-src 'self'
  X-Frame-Options: DENY
  X-Content-Type-Options: nosniff
  Referrer-Policy: no-referrer-when-downgrade
  Permissions-Policy: camera=(), microphone=()
```

---

## System Design Interview Template

```
Step 1: Clarify Requirements (5 min)
  Functional   → What does the system do?
  Non-functional → Scale? Latency? Availability? Consistency?

Step 2: Estimate Scale (2-3 min)
  Users:    1M DAU? 100M?
  Requests: 100 req/user/day × 1M = 100M req/day = ~1,160 req/s
  Storage:  1 photo × 1MB × 1M users = 1TB/day
  Bandwidth: 100M × 10KB avg = 1TB/day

Step 3: High Level Design (10 min)
  Draw: Client → CDN → Load Balancer → App Servers → Cache → DB
  Add:  Object Storage (S3) for files
  Add:  Queue for async tasks
  Add:  Separate read replicas if needed

Step 4: Deep Dive (15 min)
  → DB schema (key tables & relationships)
  → Key APIs (method, URL, request/response)
  → Bottlenecks: where does it break at scale?
  → How to shard the DB?
  → How to handle hot spots?

Step 5: Identify Failure Points & Solutions
  → Single points of failure?
  → What if cache goes down?
  → What if one region goes down?
  → Monitoring & alerting strategy
```
