


1. [article](https://medium.com/@danielabatibabatunde1/mastering-queues-in-golang-be77414abe9e)

Todo :
- Idempotency





## 1. Reliability Mechanism :

### 1. Durability : 

**Concept:** This determines the physical existence of your message. Does it live in memory (RAM) or on the hard drive (Disk)?

- **Transient (RAM):** If the Broker (RabbitMQ/Redis) restarts or crashes, the message vanishes forever.
    
- **Durable (Disk):** The message is written to the physical disk immediately. If the Broker loses power, it reloads the message from the disk upon reboot.
    

#### The Analogy: The Waiter’s Memory vs. The Order Ticket

- **Transient (RAM):** You tell a waiter, "I want a Diet Coke." They don't write it down. If they trip and fall (crash) on the way to the kitchen, they forget your order. You never get your drink.
    
- **Durable (Disk):** You order a steak. The waiter writes it on a physical pad (Disk). Even if that waiter shifts ends or takes a break, the ticket exists physically. Another waiter can pick it up.


#### Trade off :

- Performance : Disk I/O is slow. Writing every message to disk reduces throughput significantly ie, message per second.


### 2. Acknowledgement : ACK / NACK

**Concept:** The "Handshake" of completion. It prevents data loss _after_ the message leaves the broker but _before_ the work is done.

- **Auto-ACK (Dangerous):** The Broker sends the message and immediately deletes it. It assumes the network is perfect and your code has no bugs.
- **Manual ACK (Safe):** The Broker sends the message but **keeps a backup copy**. It waits. It only deletes the backup when the Consumer sends a formal "I'm done" signal.

#### The Analogy: Registered Mail vs. Regular Mail

- **Auto-ACK:** The mail carrier drops the letter in your mailbox and leaves. If a thief steals it 5 minutes later, the post office doesn't know and doesn't care. It's marked "Delivered."
    
- **Manual ACK:** The mail carrier knocks on your door. They require a signature. If you aren't home, they _take the package back to the depot_ (Queue) and try again later. They don't mark it "Delivered" until they have your signature.

#### Real-World Use Case: Image Resizing

1. **Worker** pulls a message: `resize_image_ID_123`.
2. **Broker** marks it "Unacknowledged" (Locked) but does NOT delete it.
3. **Worker** starts resizing.
    
    - _Scenario A (Success):_ Resize finishes. Worker sends `ACK`. Broker deletes the message.
        
    - _Scenario B (Crash):_ The Worker runs out of memory and crashes _during_ the resize. The connection to the Broker breaks. The Broker sees the disconnect, realizes it never got an ACK, and **re-queues** the message for another worker to try.

### 3. Visibility Timeout : Locking

**Concept:** How long does the Broker wait for an ACK before assuming the worker died?

When a worker picks up a message, that message becomes "invisible" to other workers so they don't process it simultaneously. The **Visibility Timeout** is the countdown clock on that invisibility.

#### The Analogy: The Ticketmaster Checkout Timer

You select front-row tickets for a concert.

- **The Lock:** The website gives you a timer: _"Time remaining to complete purchase: 5:00."_
- **Invisible:** During those 5 minutes, nobody else can buy those tickets. They are "reserved" (invisible) for you.
- **Timeout:** If you don't pay within 5 minutes (you crashed/distracted), the tickets are released back to the general pool for someone else to buy.


#### The Trade-off (Tuning Difficulty)

- **Too Short:** You get "phantom duplicates" where healthy workers lose the lock before finishing.
	- **Scenario:** You have a task `generate_year_end_report` that takes 10 minutes to run.
	- **Mistake:** Your Visibility Timeout is set to default (e.g., 30 seconds).
	- **The Bug:**
	    
	    1. Worker A starts the report.
	    2. At 30 seconds, Worker A is still working.
	    3. The Broker says "Time's up! Worker A must be dead."
	    4. The Broker makes the message visible again.
	    5. Worker B picks it up and _also_ starts generating the report.
	    6. **Result:** You have two workers doing the same heavy job, wasting 100% CPU.    
- **Too Long:** If a worker _actually_ crashes immediately, the system waits 10 minutes (or whatever the timeout is) before letting another worker retry. This causes a lag in recovery.



