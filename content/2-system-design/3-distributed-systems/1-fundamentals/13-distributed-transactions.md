
In a monolithic architecture, transactions are easy. You open a connection to PostgreSQL, execute three `UPDATE` statements, and run `COMMIT`. If anything fails, you run `ROLLBACK`. The database's lock manager handles all the magic, guaranteeing ACID properties.

But what happens when you split your backend into a Go-based Order Service and a Go-based Payment Service, each with their own isolated PostgreSQL database?

You can no longer use a single `COMMIT`. You are now in the brutal realm of distributed transactions. Here is exactly how we try (and often fail) to manage state across network boundaries.

### 1. Two-Phase Commit (2PC): The Legacy Approach

Two-Phase Commit attempts to provide strict ACID guarantees across multiple independent databases. It requires a central "Coordinator" to manage the transaction across the participating nodes.

- **Phase 1: The Prepare Phase (Voting).** The Coordinator tells the Order DB and the Payment DB: _"Prepare to commit this transaction. Lock your rows and write to your WAL, but do not make it visible yet."_ The databases reply with a "Yes" (I am ready) or "No" (I failed, maybe due to a constraint violation).
    
- **Phase 2: The Commit Phase.** If _all_ nodes vote "Yes," the Coordinator broadcasts a `COMMIT` command. If even _one_ node votes "No," the Coordinator broadcasts a `ROLLBACK`.
    

_(Go & Postgres Context: PostgreSQL natively supports this via the `PREPARE TRANSACTION` and `COMMIT PREPARED` SQL commands.)_

### 2. Why 2PC Blocks (And Why Modern Systems Avoid It)

While 2PC sounds bulletproof, it is a synchronous, blocking protocol. It is fundamentally fragile under network partitions.

- **The Blocking State:** Imagine the Coordinator asks the Payment DB to "Prepare." The Payment DB locks the user's balance row, writes to disk, and replies "Yes."
    
- **The Partition:** Right at that millisecond, the Coordinator server crashes before sending the final `COMMIT` or `ROLLBACK`.
    
- **The Result:** The Payment DB is now held hostage. It cannot unilaterally commit (what if the Order DB voted "No"?). It cannot unilaterally rollback (what if the Coordinator already told the Order DB to commit?). **It must hold the lock on that row indefinitely.** No other transactions can touch that user's balance until the Coordinator comes back online.
    

In a high-throughput backend, holding database locks across network calls is a death sentence for performance and availability.

### 3. Sagas: The Modern, Non-Blocking Alternative

Because 2PC sacrifices availability and latency, modern distributed systems abandon strict ACID across microservices. Instead, they embrace **Eventual Consistency** using the **Saga Pattern**.

A Saga breaks a distributed transaction into a sequence of small, independent, local transactions.

1. The Order Service opens a standard local Postgres transaction, creates the order (Status: `PENDING`), and commits immediately. The lock is released instantly.
    
2. It then publishes an event (e.g., via RabbitMQ or Kafka): `OrderCreated`.
    
3. The Payment Service consumes this event, opens its own local Postgres transaction, charges the card, commits, and publishes `PaymentSecured`.
    
4. The Order Service consumes this, updates the order to `CONFIRMED`, and commits.
    

No locks are held across the network. The system is incredibly fast and highly available.

### 4. Compensating Transactions: The "Rollback" of a Saga

The catch with Sagas is that you give up the database's magical `ROLLBACK` command. Because the Order Service already committed the `PENDING` order to disk in Step 1, you cannot simply undo it if the Payment fails in Step 3.

You must write application-level code to reverse the operation. This is a **Compensating Transaction**.

- **Analogy:** If you send an email with a typo, you can't click "un-send" (rollback). You have to send a second email apologizing for the typo (compensation).
    
- **The Flow:** If the Payment Service declines the card, it publishes a `PaymentFailed` event. The Order Service consumes this and executes a _new_ local database transaction: `UPDATE orders SET status = 'CANCELLED' WHERE id = X`.
    

### 5. The Missing Piece: Idempotency

_(Missing Context)_: Sagas rely on messaging. Networks drop messages, meaning you will inevitably have to retry them. What happens if the `OrderCreated` event is delivered to the Payment Service twice?

Without **Idempotency**, you will charge the user's credit card twice. Every single step in a Saga must be idempotent. In Go, this usually means passing a unique `Idempotency-Key` (like the Order ID) with the event. The Payment Service checks its local database: _"Have I already processed a payment for Order ID X?"_ If yes, it safely skips the work and returns success.