



**Amazon RDS** (Relational Database Service) is a **managed database service** by AWS that allows you to **create, operate, and scale** a **relational database in the cloud** without worrying about server provisioning, patching, backups, or maintenance.


## 🧱 Why Use RDS?

|Feature|Description|
|---|---|
|🛠 Fully managed|AWS handles provisioning, patching, backups, monitoring, and more|
|🚀 Easy scalability|Vertical (instance size) and horizontal (read replicas)|
|🧰 High availability|With **Multi-AZ** deployments|
|🔒 Secure|Encryption at rest and in transit, VPC isolation, IAM support|
|📊 Monitoring & logging|Built-in CloudWatch, Enhanced Monitoring, Performance Insights|
|🧪 Automated Backups|You get **point-in-time recovery** within retention period|

---

## 🛢️ Supported Database Engines

|Engine|Use Case|
|---|---|
|**MySQL**|Most popular open-source DB|
|**PostgreSQL**|Advanced features, open source|
|**MariaDB**|Community-driven fork of MySQL|
|**Oracle**|Enterprise, licensed DB|
|**SQL Server**|Microsoft stack apps|
|**Amazon Aurora**|AWS-optimized high-performance DB (MySQL & PostgreSQL compatible)|

---

## 🧠 RDS vs Aurora

| Feature       | RDS                     | Aurora                           |
| ------------- | ----------------------- | -------------------------------- |
| Compatibility | MySQL, PostgreSQL, etc. | MySQL, PostgreSQL only           |
| Performance   | Good                    | Up to 5X faster                  |
| Storage       | EBS-based               | Shared distributed storage       |
| Scaling       | Manual / limited        | Auto-scaling (Aurora Serverless) |
| Cost          | Lower                   | Slightly higher                  |

---