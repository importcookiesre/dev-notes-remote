

# 🧠 Backend Engineer Interview Roadmap (Phased)

## **PART 1 — Core Foundations (Non-negotiable)**

This is where most candidates are weak. You need sharp clarity here.

### Topics:

- **HTTP fundamentals**
    - Request/Response lifecycle
    - Methods (GET, POST, PUT, PATCH, DELETE)
    - Status codes (deep understanding, not memorization)
    - Headers, cookies, caching headers
- **Networking basics**
    - TCP vs UDP
    - DNS resolution flow
    - TLS/SSL handshake basics
- **How the web works**
    - Browser → DNS → Server → Response
    - Reverse proxy (Nginx conceptually)
- **REST API principles**
    - Resource design
    - Idempotency
    - Statelessness

---

## **PART 2 — Go Backend Engineering (Your Core Strength)**

This is where you should dominate.

### Topics:

- **Go fundamentals (interview-level depth)**
    - Goroutines & channels (very important)
    - Context propagation
    - Error handling patterns
- **Web frameworks**
    - Gin or Echo (you already use)
    - Middleware design
- **API design**
    - Request validation
    - DTO vs domain models
    - Pagination, filtering, sorting
- **Testing**
    - Unit testing
    - Integration testing
    - Mocking (testify if needed)

---

## **PART 3 — Database Mastery (Critical for Backend Roles)**

Most interviews go deep here.

### Topics:

- **PostgreSQL deep dive**
    - Indexes (B-tree, partial, composite)
    - Query optimization (EXPLAIN ANALYZE)
    - Transactions & isolation levels
    - Locks (row-level, table-level)
- **Schema design**
    - Normalization vs denormalization
    - Foreign keys vs application-level constraints
- **Migrations**
    - Tools like golang-migrate
    - Safe migrations in production
- **Connection management**
    - Pooling
    - Timeouts

---

## **PART 4 — System Design (Where You Differentiate Yourself)**

This is what separates average from strong engineers.

### Topics:

- **High-level design**
    - Load balancers
    - API gateways
    - Horizontal scaling
- **Caching**
    - Redis
    - Cache invalidation strategies
- **Messaging systems**
    - Queues (RabbitMQ)
    - Streams (Kafka basics)
- **Rate limiting**
- **Circuit breakers**
- **Idempotency in distributed systems**

---

## **PART 5 — Distributed Systems (Advanced but High ROI)**

You don’t need to be perfect, but you need working knowledge.

### Topics:

- CAP theorem
- Consistency models
- Event-driven architecture
- Saga pattern
- Retry & backoff strategies
- Dead letter queues

---

## **PART 6 — Authentication & Security**

Very common in interviews.

### Topics:

- Session vs JWT
- OAuth2 flow (you recently asked about this → good sign)
- Refresh tokens & rotation
- CSRF, XSS, SQL injection
- Password hashing (bcrypt/argon2)

---

## **PART 7 — DevOps & Production Thinking**

This is where backend engineers become “reliable”.

### Topics:

- Docker basics
- CI/CD pipelines
- Logging & monitoring
- Health checks
- Graceful shutdown

---

## **PART 8 — Behavioral + Real Experience**

This is underrated but critical.

### Topics:

- Explaining your projects deeply
- Trade-offs you made
- Debugging stories
- Failures & learnings