
### 1. Applications : 


#### **A. Reverse Proxy**
- **What it does:** Accepts traffic from the internet and forwards it to your internal servers (e.g., your Go app running on `localhost:8080`).
- **Why use it:** Security and isolation. Your Go app never touches the public internet directly.
#### **B. Load Balancer**
- **What it does:** Distributes incoming traffic across multiple instances of your application.
- **Algorithms:**
    - **Round Robin:** Connection 1 -> Server A, Connection 2 -> Server B.
    - **Least Connections:** Sends traffic to the server that is least busy.
    - **IP Hash:** Ensures a specific user always reconnects to the same server (sticky sessions).
#### **C. Static Content Server**
- **What it does:** Serves images, CSS, JavaScript, and HTML files directly from the disk.
- **Performance:** It uses kernel-level system calls (`sendfile`) to copy data from disk to network card without the CPU doing heavy lifting. It is significantly faster than Go or Python at this specific task.
#### **D. API Gateway**
- **Context:** Microservices.
- **What it does:** Acts as the single entry point for all your APIs. It can route requests based on the URL path:
    - `/api/v1/users` -> Forwards to **User Service** (Go)
    - `/api/v1/billing` -> Forwards to **Billing Service** (Python)
    - `/legacy` -> Forwards to **Old Monolith** (Java)
#### **E. Caching Layer (Content Delivery)**
- **What it does:** Memorizes the response from your backend.
- **Scenario:** If 1,000 users request the same "Top 10 Products" JSON list, NGINX asks your database **once**, caches the result for 1 minute, and serves the other 999 users from memory.
- **Impact:** Massive reduction in database load.
#### **F. Security Gatekeeper (WAF & Rate Limiting)**
- **SSL Termination:** Handles HTTPS encryption/decryption so your app doesn't have to.
- **Rate Limiting:** Prevents abuse. You can say _"Limit this IP to 10 requests per second."_ If they exceed it, NGINX blocks them with a `429 Too Many Requests` error before your app even sees the traffic.
- **IP Whitelisting/Blacklisting:** Block traffic from specific countries or known bot IPs.


### 2. Advantages :

1. **High Concurrency**: Capable of handling 10,000+ concurrent requests.
2. **HTTP Caching**: Can cache HTTP requests, reducing server load and improving response times.
3. **Reverse Proxy**: Acts as a reverse proxy, routing client requests to the appropriate server.
4. ==**Load Balancing**==: Distributes incoming requests across multiple servers to balance the load.
5. **API Gateway**: Can act as an API gateway, managing and routing API requests.
6. **Static File Serving**: Efficiently serves and caches static files like images and videos.
7. **SSL Termination**: Handles SSL certificates, providing secure connections to users.


## 4. How does NGINX handle 10k+ concurrent connections without 10k threads?

In the early 2000s, servers struggled to handle **10,000 concurrent connections** (C10k).
- **Apache:** If 10,000 users connected, Apache tried to spawn 10,000 threads. The server would run out of RAM and crash.
- **NGINX:** Solved this by using the Event Loop (explained in the previous response). It can handle 10k connections with just ~2.5 MB of RAM.

It uses a **single-threaded, event-driven architecture**. Instead of dedicating a process/thread to every connection (which blocks on I/O), NGINX uses a **Worker Process** containing an **Event Loop**.
This loop uses kernel-level notification systems (`epoll`) to monitor thousands of connection sockets simultaneously. The worker only engages with a connection when there is actual data to read or write, ensuring it never sits idle waiting for network latency or disk operations.

