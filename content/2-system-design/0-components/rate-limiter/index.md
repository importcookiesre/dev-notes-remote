
- resources : 
	- https://www.solo.io/topics/rate-limiting

## 1. What is a rate limiter ? 

Rate limiting is a technique used to control the rate at which requests are made to a network, server, or other resource.

It is used to prevent excessive or abusive use of a resource and to ensure that the resource is available to all users.




## 2. Why is rate limiting important ?

Rate limiting is important for managing network resources and ensuring the availability and performance of the networks and servers,

Here are some use cases of rate limiting : 

#### a. Prevent DoS attack and Brute force attacks.

1. **DOS**
	Rate limiter is often used to protect against Danial-of-Service (DoS) attacks, which are designed to overwhelm a network or server with huge volume of requests, making the service unavailable to legitimate users.

2. **Brute force attact** : Limiting the number of requests or attempts to access a resource can help prevent brute force attacks, where attackers try different combinations of characters to gain access to a system or application.
#### b. Manage resource utilization : 

Rate limiting can help to ensure that a network or server is not overloaded by a high volume of requests, which can negatively impact performance and availability. By limiting the rate of requests, it is possible to better manage resource utilization, prevent resource starvation, and ensure that the resources are available to all users.

#### c. Prevent abuse : 

Rate limiting can be used to prevent a single user or group from monopolizing a resource and ensure that the resouce is available to all users. It can also be used to prevent users from making excessive or unnecessary requests which waste resources and impact performance.



#### d. Reduce costs : 

Rate limiting can be used to limit a user to a fixed number of request for a time frame which will prevent users from making excessive requests. 
For example : gemini allow only limited number of image generation service to each user.

#### e. Prevent data scraping : 

 Rate limiting can help prevent data scraping by restricting the number of requests made by a single user or IP address.

## 3. Types of rate limiting : 

Rate limiting can be defined on different parameters and methods. It depends on the objective and the required level of restriction.
Here are some different types of rate limiting : 

#### 1. User based rate limits : 

User based rate limiting works by setting a limit on the number of request send by a specific user based on its api key or user id. 
This prevents a user from overwhelming a network or server after exceeding a specific threshold. 

#### 2. IP based rate limiting : 

IP-based rate limiting works by setting limits on the number of requests that can be made by a single IP address within a specific time frame. This type of rate limiting is commonly used to prevent DDoS attacks, as it can block or delay requests from a single IP address that exceed a specified threshold.

- **Advantages:** IP-based rate limiting is easy to implement and can be effective in preventing attacks from a single IP address. It can also be used to limit the amount of traffic that comes from a specific IP address to prevent resource exhaustion.
- **Limitations:** IP-based rate limiting can be circumvented by attackers who use multiple IP addresses or dynamic IP addresses. It can also block legitimate traffic from a shared IP address, such as those from a company or university.
- **Example scenario:** An online retailer implements IP-based rate limiting to prevent bots from scraping its website and stealing its product data. The rate limit is set to 10 requests per minute per IP address to prevent abusive behavior, while still allowing legitimate traffic to access the site.

#### 3. Server based rate limiting : 

Server-based rate limiting is a centralized rate limiter which is implemented on the server level. It works by setting limits on the number of requests that can be made to a specific server within a specific time frame. This type of rate limiting is commonly used to prevent resource exhaustion and ensure fair use of server resources.

- **Advantages:** Server-based rate limiting is effective in preventing resource exhaustion and ensuring fair use of server resources. It can also be used to manage peak loads and ensure that the server remains responsive during times of high traffic.
- **Limitations:** Server-based rate limiting can be circumvented by attackers who distribute their requests across multiple servers. It can also block legitimate traffic if the limit is set too low or if the server is overloaded.
- **Example scenario:** A music streaming service implements server-based rate limiting to prevent its API from being overloaded with requests during peak times. The rate limit is set to 100 requests per second per server to ensure fair use of server resources while maintaining responsiveness.

#### 4. Geography based rate limiting : 

Geography-based rate limiting works by setting limits on the number of requests that can be made from a specific geographical region within a specific time frame. This type of rate limiting is commonly used to prevent attacks from specific regions, such as those where a large number of malicious requests originate.

- **Advantages:** Geography-based rate limiting can be effective in preventing attacks from specific regions and can help to reduce the risk of resource exhaustion. It can also be used to ensure compliance with legal and regulatory requirements in specific regions.
- **Limitations:** Geography-based rate limiting can be circumvented by attackers who use proxy servers or VPNs to hide their location. It can also block legitimate traffic from a specific region, such as those from users who travel frequently or use virtual machines.
- **Example scenario:** A social media platform implements geography-based rate limiting to prevent spam and abusive behavior from a specific region known for high levels of malicious activity. The rate limit is set to 10 requests per minute per IP address originating from that region to prevent abusive behavior while still allowing legitimate traffic to access the platform.

