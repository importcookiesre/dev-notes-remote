
### 📈 What is Scaling in AWS?


> **Scaling in AWS** refers to the process of **adjusting the computing resources** (e.g., EC2 instances, containers, databases) to **match workload demand**. This can mean increasing resources when traffic grows (**scale out/up**) or reducing them when demand drops (**scale in/down**).

It’s a core feature of cloud computing—**elasticity**—which AWS provides through multiple services and techniques.


## 🧩 Types of Scaling in AWS

There are two major types:

### 1. **Vertical Scaling (Scale Up/Down)**

Increase or decrease the **size (capacity)** of a single instance.

|Direction|Example|
|---|---|
|Scale Up|Change instance type from `t2.micro` → `t3.large`|
|Scale Down|Change from `t3.medium` → `t3.micro` to reduce cost|

> ⚠️ Requires **stopping the instance** to resize in EC2.

### 2. **Horizontal Scaling (Scale Out/In)**

Add or remove **multiple instances** to handle more/less load.

|Direction|Example|
|---|---|
|Scale Out|Launch 3 more EC2 instances behind a load balancer|
|Scale In|Remove 2 EC2 instances when traffic is low|

> ✅ **Preferred for stateless, distributed applications** like web apps, APIs.

---

## 🧠 Scaling Can Be:

|Strategy|Description|
|---|---|
|**Manual Scaling**|You manually add/remove resources (change size or count).|
|**Scheduled Scaling**|Automatically scale at specific times. Example: Add capacity at 9 AM weekdays.|
|**Dynamic (Auto) Scaling**|Responds to actual load (CPU, traffic, metrics).|
|**Predictive Scaling**|Uses ML to predict future traffic and pre-scale resources.|

---
