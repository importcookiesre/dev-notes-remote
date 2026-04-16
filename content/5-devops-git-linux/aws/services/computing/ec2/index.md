- It is region specific.
- can pass commands to run after instance creation.
- connect an elastic ip with the instance for persistence ip.

Termination / stopping of ec2 : 
- remove the elastic ip
- remove the attached storage
- 
# What is EC2 in AWS?

**Amazon EC2 (Elastic Compute Cloud)** is a core service of AWS that provides **resizable compute capacity in the cloud**. It allows you to **run virtual servers (called instances)** on demand. EC2 is designed to make web-scale cloud computing easier for developers by offering flexible, secure, and scalable computing resources.


## 🧠 Why is EC2 Important?

In traditional infrastructure, if you need a server, you'd have to buy hardware, set it up, install an OS, and configure it — which is expensive and time-consuming.

With EC2, AWS gives you:

- Virtual servers within minutes
- Different OS options (Linux, Windows, etc.)
- Choice of CPU, memory, storage, and networking
- Full control over your instances
- Auto scaling and load balancing options



## 💡 Key Concepts of EC2

| Term                           | Explanation                                                                          |
| ------------------------------ | ------------------------------------------------------------------------------------ |
| **Instance**                   | A virtual server running on EC2                                                      |
| **AMI (Amazon Machine Image)** | A template with pre-installed OS and software to launch EC2 instances                |
| **Instance Types**             | Predefined hardware configurations (CPU, RAM, storage). E.g., `t2.micro`, `m5.large` |
| **EBS (Elastic Block Store)**  | Persistent block storage for your EC2 instance                                       |
| **Key Pair**                   | SSH key pair used to securely connect to Linux instances                             |
| **Security Groups**            | Virtual firewalls that control inbound/outbound traffic                              |
| **Elastic IP**                 | Static IP address you can associate with your instance                               |
| **User Data**                  | A script that runs at instance launch to configure or install software               |
| **storage**                    | Configure the type and size of storage eg EBS                                        |


## security groups : 


A **Security Group (SG)** in Amazon EC2 is a **virtual firewall** that **controls inbound and outbound traffic** to your EC2 instances. It acts at the **instance level**, not at the subnet or network level.

### 🧠 Think of it Like:

Imagine a **set of rules** attached to your EC2 instance that says:
- “Allow traffic from the internet on port 80 (HTTP)”
- “Allow SSH only from my IP”
- “Allow outbound traffic to anywhere”
---

### 🔍 Key Characteristics

|Feature|Description|
|---|---|
|**Stateful**|If you allow an incoming request, the response is automatically allowed.|
|**Instance-level**|Applies to the EC2 instance, not to the subnet or VPC.|
|**Allow-only Rules**|You can **only allow traffic** (no deny rules).|
|**Default**|By default, all inbound traffic is denied, and all outbound traffic is allowed.|
|**Applied at Launch**|You associate SGs with instances during launch or modify later.|

---

### 📦 Inbound vs Outbound Rules

|Rule Type|Direction|Example|
|---|---|---|
|**Inbound**|Traffic coming **to** the instance|Allow SSH (port 22) from your IP|
|**Outbound**|Traffic going **from** the instance|Allow all traffic to the internet|

---

![[Pasted image 20250730182428.png]]

![[Pasted image 20250730182606.png]]


# instance types : 

https://aws.amazon.com/ec2/instance-types/
