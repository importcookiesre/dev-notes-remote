
> **Amazon Web Services (AWS)** is a **cloud computing platform**. It provides a vast suite of **on-demand cloud services** that include computing power, storage, databases, networking, machine learning, security, and application development tools — all available over the internet on a **pay-as-you-go** basis.

> Launched in **2006**, AWS was the first major cloud provider and remains the market leader in the cloud industry.

What makes AWS so special ? 
> scalability, global reach, reliability, security
# services : 

#### **1. Compute**

| Service                         | Description                                                  |
| ------------------------------- | ------------------------------------------------------------ |
| **EC2 (Elastic Compute Cloud)** | Virtual servers (VMs) for running applications               |
| **Lambda**                      | Serverless computing — run code without provisioning servers |
| **ECS/EKS**                     | Manage Docker containers using AWS or Kubernetes             |
| **Elastic Beanstalk**           | PaaS for web application deployment                          |

#### **2. Storage**

|Service|Description|
|---|---|
|**S3 (Simple Storage Service)**|Scalable object storage for files and backups|
|**EBS (Elastic Block Store)**|Block storage for EC2 instances|
|**Glacier**|Low-cost archival storage|
|**FSx**|Managed file systems (Windows, Lustre, etc.)|

#### **3. Databases**

| Service      | Description                                                  |
| ------------ | ------------------------------------------------------------ |
| **RDS**      | Managed relational databases (MySQL, PostgreSQL, etc.)       |
| **DynamoDB** | NoSQL key-value and document database                        |
| **Redshift** | Data warehouse for analytics                                 |
| **Aurora**   | High-performance relational DB (MySQL/PostgreSQL compatible) |

#### **4. Networking**

|Service|Description|
|---|---|
|**VPC (Virtual Private Cloud)**|Isolated network for AWS resources|
|**Route 53**|Domain Name System (DNS) service|
|**CloudFront**|Content Delivery Network (CDN)|
|**Elastic Load Balancer**|Automatically distributes traffic|

#### **5. Security, Identity & Compliance**

|Service|Description|
|---|---|
|**IAM (Identity and Access Management)**|Control user and service permissions|
|**KMS (Key Management Service)**|Manage encryption keys|
|**CloudTrail**|Logs API activity for auditing|
|**WAF & Shield**|Web application firewall and DDoS protection|

#### **6. Developer & DevOps Tools**

|Service|Description|
|---|---|
|**CloudFormation**|Infrastructure as code|
|**CodeDeploy / CodePipeline**|Continuous deployment pipelines|
|**CloudWatch**|Monitoring and logging|
|**X-Ray**|Tracing application performance|

---

# What makes AWS so special ?

- Security
- Availavility
- Performance
- Scalability
- Flexibility



# Ways to access aws :

| Access Method         | Best For                      | Requires Credentials?          |
| --------------------- | ----------------------------- | ------------------------------ |
| AWS Console (Web UI)  | Manual management             | Yes                            |
| AWS CLI               | Scripting, automation         | Yes or IAM role                |
| AWS SDKs              | App integration               | Yes or IAM role                |
| CloudFormation/CDK    | Infrastructure as Code (IaC)  | Yes                            |
| Systems Manager (SSM) | Secure server access          | Yes (role or user)             |
| IAM Roles             | App/service-level permissions | No (credentials auto-assigned) |
| API (HTTPS)           | Custom integrations           | Yes                            |

## Aws cli : 

The AWS Command Line Interface (AWS CLI) is an open source tool that enables you to interact with AWS services using commands in your command-line shell. With minimal configuration, the AWS CLI enables you to start running commands that implement functionality equivalent to that provided by the browser-based AWS Management Console from the command prompt in your terminal program

https://aws.amazon.com/cli/

