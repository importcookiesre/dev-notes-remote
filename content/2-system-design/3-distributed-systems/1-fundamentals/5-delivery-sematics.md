
You are throwing a message over a network doesn't guarentees it will reach or you get a response as the network is unrealiable.
We have to decide what our system should do when a message inevitably gets lost.



## 1. At-Most-Once : fire and forget

**What it means:** The client sends a message exactly one time. If it arrives, great. If a network switch drops it, or the server crashes before processing it, the message is gone forever. The client **never retries**.
- **Guarantees:** The server will process the message either 0 times or 1 time. It will never process it twice.
- **When to use it:** When performance and low latency are strictly more important than data completeness.
- **Real-World Backend Example:** Sending CPU metrics to a monitoring dashboard, or streaming frames of a live video. If you lose a frame, you don't want the system to freeze and retry sending a 5-second-old frame; you just want the newest one.

## 2. At-Least-Once : Retry Until ACK.

**What it means:** The client sends a message. It sets a timer. If it does not receive a positive acknowledgment (ACK) from the server before the timer expires, the client assumes the message was lost and **retries**. It will keep retrying until it gets a confirmed success.

- **Guarantees:** The server will process the message 1 time, 2 times, or 100 times. It will never process it 0 times.
    
- **When to use it:** When data loss is unacceptable. This is the default semantic for almost all backend systems, databases, and message queues.
    
- **The Danger:** It creates duplicates.


## 3. If a client times out but the server processed the request, what happens?

**The Sequence of Events:**

1. A user clicks "Buy". The Go client sends `POST /checkout` to the Payment Server.
    
2. The Payment Server receives it, successfully deducts $50 in PostgreSQL, and replies with `HTTP 200 OK`.
    
3. Before the `200 OK` reaches the client, the network drops the packet (or the client's internal `context.WithTimeout` expires).
    
4. **What happens:** The client sees an error ("Request Timeout"). The user sees a red "Failed" banner on their screen. But the server _already moved the money_.
    

If your system uses raw **At-Least-Once** semantics, the client (or the user) will blindly retry the request. The server will receive it again, treat it as a brand-new request, and deduct _another_ $50. You have now double-charged the customer.

To solve this, we must build an illusion.

## 3. Exactly once :

**What it means:** The client guarantees the message is delivered, and the server guarantees the business logic is executed one and only one time, regardless of how many times the network drops packets or the client retries.

- **The Reality:** Pure mathematical "Exactly-Once Delivery" over an unreliable network is theoretically impossible (due to the _Two Generals' Problem_).
    
- **The Practical Solution:** Backend engineers achieve **Exactly-Once Processing** by combining At-Least-Once Delivery with **Idempotency**.


### How to Build Exactly-Once (Idempotency)

To fix the double-charge scenario above, the system must recognize when a request is a duplicate.

1. **The Client generates a unique ID:** Before sending the request, the client generates a UUID (e.g., `Idempotency-Key: 12345`) and attaches it to the HTTP header.
    
2. **The Server checks its memory:** When the Go API receives the request, it first checks a PostgreSQL table or Redis cache for that key.
    
3. **First Attempt:** The key is not found. The server locks the key in the database, processes the $50 charge, stores the `200 OK` response alongside the key, and returns the response.
    
4. **The Network Drops the Response.**
    
5. **The Retry:** The client times out and safely retries the exact same request with the _same_ `Idempotency-Key: 12345`.
    
6. **The Intercept:** The server receives the retry. It checks the database, sees `12345` already exists and was successfully processed. **It completely skips the business logic (no money is moved)**, grabs the cached `200 OK` response, and hands it back to the client.
    

The client retried (At-Least-Once), but the server state only changed once (Exactly-Once).