

# How we can design Restful APIs for microservices ?


We should focus on the business entities that we expose **APIs** for our application. So that means we should organize our resources according to business entities and **expose** them properly via **APIs**.

Lets think about our **e-commerce application**, the primary entities might be customers and orders. When creating an order, we basically send **HTTP POST** request with contains customer and order detail information's. And return back to **HTTP response** that including **200 OK** success response.

So how we can handle this kind of Restful API ? The best practice is the resource URIs should be based on nouns (the resource) and not verbs.

**For example :**  
[https://eshop.com/orders](https://eshop.com/orders) // **Correct**

[https://eshop.com/create-order](https://eshop.com/create-order) // **Wrong**

So now lets think about the e-commerce application and our Customer and Order entities, resources.
![[4.webp]]

Since we are using http protocol, You can find Different Resource Urls are indicating with http verbs like get-put-post and delete.

See the table we have **/customers** main resource. and we can filter by adding **/1** and **/orders** by following REST principles

## Example : 


In a **microservices architecture**, microservices don’t share the same code base and don’t share data stores. Instead of that, they communicate through APIs for data operations.
![[5.webp]]

If we look at the image, the **Shopping Cart service** requests information about a customer from the **Customer service**. The Customer service retrieve data with using **Repository classes** and return **Customer** entity model as a **JSON** object in an **HTTP** response. So this provide to isolation of services.


# REST api design for microservices : 




### ## 1. The API Gateway: Your Front Door 🚪

The API Gateway is the most critical pattern for microservices. It's a server that acts as a unified, public-facing entry point for all your backend services.

**Its Responsibilities:**

- **Routing:** When a client sends a request like `GET /api/v1/orders/123`, the gateway knows to route it to the internal `Order Service`. The client never needs to know the specific address of the `Order Service`.
    
- **Authentication & Authorization:** The gateway is the perfect place to handle security. It can validate a client's token (e.g., a JWT) and ensure they have permission before forwarding the request. This means individual services don't need to repeat complex security logic.
    
- **Request Aggregation:** A client might need information from multiple services to build a single view. Instead of making the client call the `User Service`, `Order Service`, and `Product Service` separately, the client can make one call to the gateway. The gateway then calls the internal services and combines their responses.
    
- **Cross-Cutting Concerns:** It can handle rate limiting, caching, logging, and SSL termination for all services in one place.
    

---

### 2. Define Clear Service Boundaries & Ownership

The golden rule of microservices is **single responsibility and loose coupling**.

- **Data Ownership:** Each microservice must own its specific domain and data. For example, the `User Service` is the _only_ service allowed to directly access the user database.
    
- **Communication Over APIs:** If the `Order Service` needs to know a user's shipping address, it **must not** query the user database directly. Instead, it must make a REST API call to the `User Service` (e.g., `GET /users/456`). This enforces separation and allows services to evolve independently.
    

---

### ## 3. Smart URI Design and Versioning 🚀

Consistency is key when you have dozens of services.

- **URI Structure:** Design clean, predictable URIs. A good practice is for the gateway to expose a path that includes the service or domain name.
    
    - Client calls: `https://api.yourcompany.com/user-service/users/123`
        
    - Client calls: `https://api.yourcompany.com/order-service/orders/456`
        
- **API Versioning:** It's inevitable that your APIs will change. The most common and clearest way to handle this is with **URI versioning**, managed at the API Gateway.
    
    - **Example:** `https://api.yourcompany.com/v1/users/123`
        
    - When you release a new, breaking change, you can introduce a `v2` endpoint (`/v2/users/123`) while keeping `v1` active for older clients.
        

---

### ## 4. Handle Inter-Service Communication ⛓️

Services need to talk to each other. There are two primary models:

1. **Synchronous (REST calls):** The `Order Service` calls the `Inventory Service` and waits for a response. This is simple and direct but can lead to **cascading failures** (if the Inventory Service is down, the Order Service is impaired). You must implement patterns like **circuit breakers** to prevent this.
    
2. **Asynchronous (Event-Driven):** The `Order Service` publishes an `OrderCreated` event to a message broker (like RabbitMQ or Kafka). The `Notification Service` and `Inventory Service` subscribe to this event and react accordingly, without the `Order Service` having to wait. This is more resilient and decoupled but introduces eventual consistency.
    

**A good approach is to use both:** use synchronous REST calls for queries (getting data) and asynchronous events for commands or notifications.

---

### ## 5. Service Discovery

In a dynamic microservices environment (especially with containers), you can't rely on static IP addresses for your services. A **Service Discovery** tool is essential.

- **How it works:** When an instance of the `Product Service` starts, it registers its location (IP address and port) with a central **Service Registry** (like Consul or Eureka).
    
- When the API Gateway or another service needs to call the `Product Service`, it first asks the registry for a healthy instance's address. This makes the system resilient to failures and allows for easy scaling.