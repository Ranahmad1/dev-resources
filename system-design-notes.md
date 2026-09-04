# System Design Notes

> Essential concepts every developer should know.

## Core Concepts

### Scalability
```
Vertical Scaling (Scale Up)
→ Add more power to existing server (more CPU, RAM)
→ Simple but has a limit (single point of failure)
→ Example: Upgrade 8GB RAM server to 32GB

Horizontal Scaling (Scale Out)
→ Add more servers and distribute load
→ More complex but unlimited potential
→ Example: Add 10 more web servers behind a load balancer
```

### Load Balancer
```
What: Distributes traffic across multiple servers
Why: Prevents single server overload, high availability

Algorithms:
• Round Robin       → Send to each server in order
• Least Connections → Send to least busy server
• IP Hash           → Same user always hits same server
• Weighted          → More traffic to stronger servers

Tools: Nginx, HAProxy, AWS ALB, Cloudflare
```

### Caching
```
What: Store frequently accessed data in fast memory
Why: Reduce DB load, faster response times

Levels:
1. Browser Cache      → Static files (CSS, JS, images)
2. CDN Cache          → Global edge servers
3. Application Cache  → In-memory (Redis, Memcached)
4. Database Cache     → Query cache

Strategies:
• Cache Aside   → App checks cache → miss? load from DB → store in cache
• Write Through → Write to cache AND DB simultaneously
• Write Back    → Write to cache only, sync to DB later

Cache Invalidation:
• TTL (Time To Live)  → Expire after X seconds
• Event-based         → Invalidate when data changes

Redis Use Cases:
→ Session storage
→ Rate limiting
→ Pub/Sub messaging
→ Leaderboards
→ Real-time analytics
```

### Database Design
```
SQL (Relational):
• Structured data with clear schema
• ACID compliant (Atomicity, Consistency, Isolation, Durability)
• Best for: Banking, E-commerce, ERP
• Examples: MySQL, PostgreSQL, Oracle

NoSQL:
• Flexible schema, horizontal scaling
• Types:
  - Document Store  → MongoDB (JSON-like docs)
  - Key-Value       → Redis, DynamoDB
  - Column Family   → Cassandra (time-series, IoT)
  - Graph           → Neo4j (social networks)
• Best for: Real-time apps, Big Data, Social media

Indexing:
→ Speeds up reads but slows writes
→ Index frequently queried columns (WHERE, JOIN, ORDER BY)
→ Avoid over-indexing

Database Normalization:
• 1NF → Atomic values, no repeating groups
• 2NF → No partial dependencies on composite key
• 3NF → No transitive dependencies

Denormalization:
→ Intentionally add redundancy for read performance
→ Used in data warehouses and analytics
```

### API Design Patterns
```
REST:
→ Stateless, HTTP-based, resource-oriented
→ Best for: Public APIs, CRUD operations

GraphQL:
→ Client requests exactly what it needs
→ Single endpoint
→ Best for: Complex UIs with varying data needs
→ Example: Facebook, GitHub API v4

gRPC:
→ Protocol Buffers (binary, fast)
→ Streaming support
→ Best for: Microservices internal communication

WebSocket:
→ Persistent bidirectional connection
→ Best for: Chat, live updates, gaming

Webhook:
→ Server pushes to your URL when event happens
→ Best for: Payment callbacks, CI/CD triggers
```

### Message Queues
```
What: Async communication between services
Why: Decouple services, handle traffic spikes, retry on failure

Flow:
Producer → [Queue] → Consumer

Tools:
• RabbitMQ   → Traditional message broker
• Apache Kafka → High-throughput event streaming
• AWS SQS     → Managed cloud queue
• Redis Pub/Sub → Lightweight pub/sub

Use Cases:
→ Email/notification sending
→ Image processing
→ Order processing
→ Event logging
→ Microservice communication
```

### Microservices vs Monolith
```
Monolith:
✅ Simple to develop and deploy (initially)
✅ Easy debugging (single codebase)
❌ Hard to scale individual parts
❌ One bug can crash everything
❌ Tech stack locked

Microservices:
✅ Independent scaling
✅ Independent deployment
✅ Tech flexibility per service
✅ Fault isolation
❌ Complex infrastructure
❌ Network latency between services
❌ Distributed system challenges

Start with Monolith → Split into Microservices when needed
```

### CDN (Content Delivery Network)
```
What: Distributed servers cache static content globally
Why: Faster load times for users worldwide

How:
1. User requests file
2. CDN finds nearest edge server
3. Edge serves cached file
4. If not cached → fetch from origin → cache → serve

Best for: Images, videos, CSS, JS, HTML
Providers: Cloudflare, AWS CloudFront, Fastly
```

### CAP Theorem
```
Distributed systems can only guarantee 2 of 3:

C = Consistency      → All nodes see same data
A = Availability     → Always responds to requests
P = Partition Tolerance → Works despite network failures

Real-world picks:
• CP → MongoDB, HBase (bank transactions)
• AP → Cassandra, DynamoDB (social feeds)
Note: P is usually non-negotiable → choose C or A
```

### Rate Limiting
```
Why: Prevent abuse, protect resources, fair usage

Algorithms:
• Token Bucket    → Add tokens over time, request consumes token
• Leaky Bucket    → Queue requests, process at fixed rate
• Fixed Window    → Count per time window (e.g., 100 req/min)
• Sliding Window  → Rolling time window, more accurate

Implementation: Nginx, Redis + middleware, API Gateway

HTTP Response:
429 Too Many Requests
Retry-After: 60
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 0
```

### Security Basics
```
Authentication vs Authorization:
• Authentication → Who are you? (login)
• Authorization  → What can you do? (permissions)

Common Attacks:
• SQL Injection    → Use prepared statements / ORM
• XSS             → Escape output, Content-Security-Policy
• CSRF            → Use CSRF tokens
• MITM            → Use HTTPS (TLS/SSL)
• DDoS            → Rate limiting, CDN, WAF
• Brute Force     → Rate limit login, CAPTCHA, lockout

Password Security:
→ Never store plain text
→ Hash with bcrypt / argon2
→ Salt before hashing

JWT (JSON Web Token):
Header.Payload.Signature
→ Stateless authentication
→ Store in HttpOnly cookie (not localStorage)
→ Short expiry + refresh tokens
```

### System Design Interview Template
```
1. Clarify Requirements (5 min)
   → Functional: What the system does
   → Non-functional: Scale, latency, availability

2. Estimate Scale (2 min)
   → Users: 1M DAU?
   → Requests: 100 req/user/day = 100M req/day
   → Storage: 1KB/record × 100M = 100GB/day

3. High Level Design (10 min)
   → Draw: Client → Load Balancer → App Servers → DB
   → Add: Cache, CDN, Queue where needed

4. Deep Dive (15 min)
   → DB schema
   → Key API endpoints
   → Bottlenecks and solutions

5. Identify Bottlenecks
   → Single points of failure?
   → How to scale?
   → Monitoring & alerting?
```
