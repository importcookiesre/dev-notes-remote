



## 1. Architecture : master and worker


#### A. The Master Process
- **Role:** The Manager.
- **Responsibilities:** It reads and validates configuration files (`nginx.conf`), manages the worker processes (starts them, stops them, reloads them seamlessly), and handles logging.
- **Privilege:** It runs as `root` (usually) to bind to privileged ports (like 80 or 443), but it does **not** handle any network traffic itself.
#### B. The Worker Processes
- **Role:** The Laborers.
- **Responsibilities:** These handle the actual network connections, read data from disk, and communicate with backend servers.
- **Count:** usually set to `auto` (equal to the number of CPU cores).
- **Privilege:** They run as a non-privileged user (e.g., `www-data`) for security.
- **Key Concept:** Each worker is single-threaded and runs independently. They do not share memory; they share "listening sockets."


## 2. Event Loop & Non Blocking I/O

This is the most critical part to understand. How does **one** single-threaded worker handle 10,000 connections?
It uses a mechanism called **I/O Multiplexing**.

#### The Cycle (The Event Loop):

1. **Accept:** The worker accepts a new connection (e.g., Client A).
2. **Register:** Instead of reading the request immediately and blocking if the data isn't there, the worker tells the OS kernel: _"Hey, let me know when Client A sends data."_
3. **Loop:** The worker immediately moves to the next task or connection. It is **Non-Blocking**.
4. **Notification:** When Client A sends the HTTP headers, the Kernel triggers an event (via `epoll`). The worker wakes up, reads the data quickly, parses it, and decides what to do.


