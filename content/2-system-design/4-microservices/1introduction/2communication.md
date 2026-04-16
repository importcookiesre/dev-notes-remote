
Reference :
- https://medium.com/@joudwawad/a-guide-to-communication-styles-in-microservices-architecture-9a8ae4bc21b2
- https://medium.com/design-microservices-architecture-with-patterns/microservices-communications-f319f8d76b71


When we are talking about Monolithic applications, we said that the **communication** in Monolithic applications are **inter-process communication**. So that means it is working on **single process** that invoke one to another by **using method calls.** You just create class and call the method inside of target module. All running the same process.


One of the biggest challenge when moving to **microservices**-**based application** is changing the communication mechanism. Because microservices are **distributed** and **microservices communicate** with each other by **inter**-**service communication** on network level. Each microservice has its own **instance** and **process**. Therefore, services must interact using an **inter**-**service communication** protocols like **HTTP**, **gRPC** or **message brokers AMQP** protocol.



# Microservices Communication Types — Sync or Async Communication

![[1_63py5DO08sV7rKOcS8mK-Q.webp]]


| Category         | Description                                                     | Examples        |
| ---------------- | --------------------------------------------------------------- | --------------- |
| **Synchronous**  | Real-time, blocking communication. Caller waits for a response. | HTTP/REST, gRPC |
| **Asynchronous** | Non-blocking communication via messages or events.              | Kafka, RabbitMQ |

## synchronous communication : 

**Synchronous communication** is a request-response style of communication where the caller service is blocked until it receives a response from the callee service

### 🔧 **How it Works:**

Imagine Service A needs some data from Service B.

1. Service A sends an HTTP/gRPC request to Service B.
2. Service B processes the request and returns a response.
3. Only then does Service A proceed with its workflow.


### **Common Protocols Used:**

- **HTTP/HTTPS (REST APIs)** – most widely used.
- **gRPC** – efficient for internal communication.
- **WebSockets** (less common for RPC, more for streaming).


## Asynchronous communication : 

In **asynchronous communication**, a service sends a request to another service but **does not wait** for a response. Instead, it continues its work and assumes that the response will arrive **later** (if needed).
==So the key point here is that, the client should not have== ==**blocked**== ==a== ==**thread**== ==while waiting for a response.==

The most popular protocol for this Asynchronous communications is **AMQP (Advanced Message Queuing Protocol)**. So with using **AMQP protocols**, the client sends the message with using message broker systems like **Kafka** and **RabbitMQ queue**. The message producer usually does not wait for a response. This message consume from the subscriber systems in **async** way, and no one waiting for response suddenly.


An **asynchronous communication** also divided by 2 according to implementation. An asynchronous systems can be implemented in a **one-to-one(queue) mode** or **one-to-many (topic) mode.**

In a **one-to-one(queue)** implementation there is a single producer and single receiver. But in **one-to-many (topic)** implementation has Multiple receivers. Each request can be processed by zero to multiple receivers. **one-to-many (topic)** communications must be asynchronous.

**Kafka** and **RabbitMQ** is the best tools for this operations.


