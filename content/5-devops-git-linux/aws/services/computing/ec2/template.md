

### 📘 What is a Launch Template in AWS EC2?

**Definition (formal):**

> A **Launch Template** is a reusable configuration template in Amazon EC2 that defines parameters required to launch an instance, such as the AMI, instance type, key pair, security groups, network settings, storage volumes, and user data.

It simplifies the process of launching instances and ensures consistency across deployments—especially helpful when launching many instances, using **Auto Scaling**, or **creating spot fleets**.

---

## 🧱 Why Use Launch Templates?

Imagine you frequently launch EC2 instances with the same setup: same AMI, instance type, EBS settings, user data, etc.

Instead of configuring everything manually each time, you create a **Launch Template** once and reuse it whenever needed.

This helps in:

|Use Case|Benefit|
|---|---|
|Frequent launches|Avoids repetitive manual config|
|Auto Scaling Groups (ASG)|Uses launch templates to scale instances|
|Spot instances/Fleets|Simplifies spot configuration|
|Version control|Each update creates a new version|

---

## ⚙️ Components of a Launch Template

Here’s what you can specify in a Launch Template:

|Parameter|Description|
|---|---|
|**AMI ID**|The Amazon Machine Image to use|
|**Instance Type**|e.g., `t2.micro`, `t3.medium`, etc.|
|**Key Pair**|SSH key for connecting|
|**Security Groups**|Firewall rules|
|**User Data**|Shell script to run at first boot (cloud-init)|
|**Storage Volumes**|EBS volume size, type, encrypted or not|
|**IAM Role**|Attach a role to instance for accessing other AWS services|
|**Tag Specifications**|Add tags like `Name=WebServer`|
|**Network Interfaces**|Set subnet, IP address, public IP, etc.|
|**Monitoring**|Enable/disable CloudWatch detailed monitoring|

---

## 🛠 How to Create a Launch Template (Step-by-Step)

1. **Go to EC2 Dashboard** → Left Sidebar → **Launch Templates**
2. Click **Create Launch Template**
3. Fill in:
    - **Template name**
    - **AMI ID**
    - **Instance type**
    - **Key pair**
    - **Security group**
    - **Storage settings**
    - (Optional) **User data script** to automate setup
4. Click **Create Template**

✅ That’s it! You now have a reusable template.

---
## 🚀 How to Use a Launch Template

You can launch instances using this template:
1. Go to **EC2 > Launch Templates**
2. Select the template
3. Click **Actions > Launch instance from template**
4. You can override some parameters if needed
5. Launch!
---

## 🧬 Versioning in Launch Templates

Every time you **modify a launch template**, AWS creates a new **version**. This allows you to:
- Rollback to older versions
- Test configurations with different versions
- Use specific versions in Auto Scaling Groups or Spot Fleets

> Example:  
> `LaunchTemplate: WebServerTemplate v1` uses `t2.micro`  
> `v2` uses `t3.micro`  
> `v3` adds new user data for Apache install

You can set:

- **Default version** (used if no version specified)
- **Specific version** for different deployment groups

---