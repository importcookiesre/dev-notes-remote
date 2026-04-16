

In AWS, **IAM** stands for **Identity and Access Management**.

IAM is a service that helps you securely control access to AWS resources.
It allows you to manage users, roles , and permission to define who can access what within your AWS environment.

- It is a global service.
- Root account created by default, shouldn't be used or shared.

### What IAM Does:

IAM lets you:

- **Create and manage users and groups** in your AWS account.
- **Assign permissions** to allow or deny access to AWS resources.
- Use **roles** to delegate access without sharing credentials.
- Apply **policies** (written in JSON) to define what actions are allowed or denied for different entities.

---

## Users vs Groups

Users : Give permission independently. 
Group : Create different group like dev team , ops team with dedicated permission and add users based on roles.

![[Pasted image 20250730162403.png]]