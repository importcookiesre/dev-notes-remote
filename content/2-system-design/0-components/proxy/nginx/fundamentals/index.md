- Nginx is a web server that takes requests from the clients and deligatest it.
- It uses a single thread and an event driven architecture.
- Nginx does not runs any application code.
- NGINX consumes almost zero CPU while "waiting" for a database or a client. It effectively goes to sleep until the OS kicks it.
- If a client on a slow mobile connection requests a large JSON payload, NGINX retrieves it from your Go backend in 1ms (freeing up your Go thread immediately) and then slowly trickles the data to the mobile user over 2 seconds. This is **Buffering**, and it saves your backend from being clogged by slow clients.

## NGINX as a :  reverse proxy vs application server

- Nginx does not runs any application code, it just
	- Sits in front of the application server.
	- Handles the "boring" I/O work: SSL termination, compression (gzip), serving static files (images/css), and buffering slow clients.
	- _Heavy I/O usage._

**Why this split matters:** If a client on a slow mobile connection requests a large JSON payload, NGINX retrieves it from your Go backend in 1ms (freeing up your Go thread immediately) and then slowly trickles the data to the mobile user over 2 seconds. This is **Buffering**, and it saves your backend from being clogged by slow clients.



## NGINX vs Apache

| Feature             | Apache HTTP Server                                                                           | NGINX                                                                                                                   |
| ------------------- | -------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **Architecture**    | **Process/Thread-based.** Creates a new thread/process for every connection. Blocking I/O.   | **Event-driven & Asynchronous.** Single thread handles thousands of connections. Non-blocking I/O.                      |
| **Static Files**    | Slower. Consumes thread overhead just to serve a simple image.                               | **King of Static.** Uses very little RAM and serves files up to 2x faster.                                              |
| **Dynamic Content** | **Native.** Can process PHP/Python _inside_ the server itself (e.g., `mod_php`).             | **External only.** Must pass dynamic requests to a separate process (e.g., Go binary, PHP-FPM).                         |
| **Configuration**   | **Decentralized (`.htaccess`).** Allows config changes per folder without restarting server. | **Centralized (`nginx.conf`).** High performance because it doesn't scan folders, but less flexible for shared hosting. |
| **Memory Usage**    | High. Usage grows linearly with connections.                                                 | Low and constant. Usage remains stable even under high load.                                                            |
In the early 2000s, servers struggled to handle **10,000 concurrent connections** (C10k).
- **Apache:** If 10,000 users connected, Apache tried to spawn 10,000 threads. The server would run out of RAM and crash.
- **NGINX:** Solved this by using the Event Loop (explained in the previous response). It can handle 10k connections with just ~2.5 MB of RAM.