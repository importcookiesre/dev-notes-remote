
## 1. Synchronous vs. Asynchronous


### 1. Synchronous (Blocking) 

1. **Synchronous communication** is a blocking interaction where the caller waits for the callee to finish processing and return a response.
2. **In Backend:** A standard HTTP REST request. Service A calls Service B. Service A freezes (blocks) or spins waiting until Service B sends a response.
3. **Risk:** If Service B is slow, Service A becomes slow. If Service B is down, Service A errors out immediately. The failure cascades.

### 2. Asynchronous (Non-blocking) :

1. **Asynchronous communication** is a non-blocking interaction where the caller submits work and continues execution without waiting for completion.
2. **In Backend:** Service A sends a message to a Queue. It gets an immediate "Message Received" confirmation from the Broker and moves on.
3. **Benefit:** Service A is fast because it never waits for the actual heavy lifting to be done.


### 3. Temporal Decoupling :

1. **Temporal decoupling** means producers and consumers do not need to be online or running at the same time to exchange information.
2. In a monolithic or synchronous system, the Producer and Consumer are **time-bound**. They must be online _at the exact same second_ for the transaction to succeed.
3. **The Problem:** If your "Invoice PDF Generator" service crashes at 2:00 AM, and a user tries to buy something at 2:01 AM, the purchase fails. You lose money.
4. **The Solution (Temporal Decoupling):** The checkout service places a "Generate PDF" message in the queue.
	
	- If the PDF service is crashed, the message sits safely in the queue.
	- At 2:15 AM, the PDF service restarts. It looks at the queue, sees the pending message, and processes it.
	- **Result:** The user purchased successfully at 2:01 AM. They just got their email a bit later. The system was resilient to failure.

### 4.  Eventual Consistency : 

1. This is the trade-off you make when you choose Temporal Decoupling.
- **Immediate Consistency (The "Old" Way):** When a user clicks "Buy," the inventory count in the database is updated _instantly_ before the screen reloads. The data is perfect, but the database is locked during the update.
	
- **Eventual Consistency (The "Async" Way):** You accept that for a brief period (milliseconds to seconds), the system state might be slightly out of sync across different services.
	
	- **Scenario:** User updates their profile photo.
		
	- **Action:** You return "Success" immediately to the UI.
		
	- **Background:** A message goes to a queue to resize the image and update the CDN.
		
	- **The Gap:** For 2 seconds, the user might still see their _old_ photo until the worker finishes. The system is inconsistent for 2 seconds, but eventually, it becomes consistent.

### 5. Throttling / Load Leveling : 

This protects your database and consumers from being crushed by success (high traffic).

- **The Spike:** Imagine your app goes viral. You suddenly get 10,000 requests per second.
	
- **Without a Queue:** Your database tries to open 10,000 connections. It runs out of RAM and crashes. The whole site goes down.
- **With a Queue (Load Leveling):** The API accepts the 10,000 requests and dumps them into a Queue (which is very fast).
	
	- Your Consumers (Workers) are configured to only process 50 messages per second.
		
	- The Queue fills up (acting as a buffer), but the database only sees a steady stream of 50 operations per second.
		
	- **Result:** The system is slower to finish all tasks, but it **survives** the spike without crashing.

