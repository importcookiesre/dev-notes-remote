- It charges if ip is created and not associated with any instance.
	- remove it if you are not using.
# Elastic IP

### 🌐 What is an Elastic IP in AWS?


> An **Elastic IP address** is a **static, public IPv4 address** designed for **dynamic cloud computing**. It is associated with your AWS account and can be **attached to or detached from EC2 instances** as needed.

An **Elastic IP (EIP)** is a **permanent public IP** that you can use **even if your EC2 instance stops, fails, or is replaced**. You can detach it from one instance and attach it to another.

Without an Elastic IP, a regular EC2 **public IP changes every time** the instance stops and starts.

## Why Use an Elastic IP?

| Use Case               | Benefit                                                         |
| ---------------------- | --------------------------------------------------------------- |
| Web server             | Keep the same IP so your domain name or users never lose access |
| Failover               | Quickly reassign the IP to a backup instance                    |
| Static DNS mapping     | Map your DNS record to a fixed IP                               |
| Dynamic infrastructure | Easily swap backend servers without updating IPs                |

