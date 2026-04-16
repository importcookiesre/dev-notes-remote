
## 🧱 Types of Load Balancers in AWS

AWS provides **4 types** of Elastic Load Balancers:

| Load Balancer Type                  | Layer                | Use Case                                                  |
| ----------------------------------- | -------------------- | --------------------------------------------------------- |
| **Application Load Balancer (ALB)** | Layer 7 (HTTP/HTTPS) | Web apps, routing by URL/path/host headers                |
| **Network Load Balancer (NLB)**     | Layer 4 (TCP/UDP)    | Low latency, high throughput apps (e.g., gaming, VoIP)    |
| **Gateway Load Balancer (GWLB)**    | Layer 3/4 (IP)       | For inserting third-party security appliances (firewalls) |
| **Classic Load Balancer (CLB)**     | Layer 4 & 7          | Legacy only – use ALB/NLB instead                         |
| dfghdfjg                            | sdjkhgfsdjk          |                                                           |
|                                     |                      |                                                           |

---

### 🔁 1. Application Load Balancer (ALB)

✅ Best for web apps

|Feature|Description|
|---|---|
|Protocols|HTTP, HTTPS|
|Intelligent Routing|Based on host/path (e.g., `/api`, `/user`, `login.mysite.com`)|
|Target Types|EC2, containers, IPs|
|Use Case|Microservices, modern web apps, path-based routing|

Example:

- `/api/*` → forwards to backend microservice A
    
- `/user/*` → forwards to microservice B
    

---

### 🌐 2. Network Load Balancer (NLB)

✅ Best for performance-critical apps

|Feature|Description|
|---|---|
|Protocols|TCP, UDP, TLS|
|Low Latency|Millions of requests per second|
|IP Address Targeting|Yes|
|Static IP Support|Yes (Elastic IPs)|
|Use Case|Financial apps, gaming, video streaming|

---

### 🛡️ 3. Gateway Load Balancer (GWLB)

✅ Best for integrating security

|Feature|Description|
|---|---|
|Traffic Steering|Sends traffic to 3rd-party appliances|
|Use Case|Deploying firewalls, IDS/IPS in the cloud|
|Protocol|IP Forwarding (GENEVE protocol)|

---

### 🧓 4. Classic Load Balancer (CLB)

✅ Legacy only (not recommended for new apps)

Supports basic L4 (TCP) and L7 (HTTP/HTTPS), but lacks modern features like path-based routing, target groups, WebSockets, etc.

---
