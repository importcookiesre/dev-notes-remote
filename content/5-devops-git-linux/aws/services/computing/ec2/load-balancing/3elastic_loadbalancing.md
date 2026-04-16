

![[Pasted image 20250730222824.png]]


# 🔍 Why Use ELB?

Without a load balancer, your traffic might hit just one server. That’s risky:
- If the server crashes → downtime.
- If traffic spikes → performance drops.

With ELB:
- Traffic is **evenly distributed**
- Automatically **reroutes traffic** from unhealthy instances
- Works **seamlessly with Auto Scaling**
- Supports **SSL termination**, **sticky sessions**, and **health checks**
- high availability : if one region fails, it will send request to other regions.

![[Pasted image 20250730223133.png]]


