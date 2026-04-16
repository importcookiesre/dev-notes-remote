
HTTP != REST 
	- REST is one of the feature of HTTP



# rest-api

**REST (Representational State Transfer)** is a service structure that enables easy and fast communication between client and server. Roy Fielding introduced and developed REST in his doctoral thesis in 2000. It was developed as an alternative to SOAP and WSDL based Web services. REST runs on HTTP.

REST allows applications to communicate with each other by carrying **JSON** data between the client and server.

It is faster, more efficient in sending and receiving data over its alternatives.

## Features of REST

When we look at the constraints of the REST architecture, we come across six items:

- Stateless
- Uniform Interface
- Cacheable
- Client-Server
- Layered System
- Code on Demand

For an API to be truly RESTful, it must follow six guiding principles:

1. **Client-Server Architecture**: The client (e.g., a mobile app or browser) and the server are separate. The client handles the user interface, and the server handles the data and business logic. They communicate over a network.
    
2. **Stateless**: This is a crucial rule. Each request from the client to the server must contain all the information the server needs to understand and fulfill it. The server does not store any session state about the client between requests. This makes the system more reliable and scalable.
    
3. **Cacheable**: Responses from the server should indicate whether they can be cached by the client. Caching improves performance and reduces server load.
    
4. **Uniform Interface**: This simplifies the architecture by requiring all components to speak the same language. It involves:
    
    - **Resource-Based URIs**: Identifying resources with a unique URI (e.g., `/users/123`).
        
    - **Standard HTTP Methods**: Using standard HTTP verbs to manipulate resources (GET, POST, PUT, DELETE).
        
    - **Self-Descriptive Messages**: Responses (like JSON) should be self-contained and describe the resource.
        
5. **Layered System**: The client doesn't need to know if it's connected directly to the end server or to an intermediary (like a load balancer or a proxy). This allows the system architecture to evolve without affecting the client.
    
6. **Code on Demand (Optional)**: Servers can temporarily extend client functionality by sending executable code, like JavaScript. This is the only optional constraint.
## What is RESTful apis ?

Web services that use **REST** architecture are called **RESTful** services. **RESTful** systems generally communicate over **HTTP** protocol with **HTTP** methods (GET, POST, PUT, DELETE etc.) used by Web Browsers to transfer pages.

## HTTP methods : 
![[http.webp]]

- Patch : Used to update a single piece of data.




# gRPC :



## what is grpc ? 

**gRPC (gRPC Remote Procedure Calls)** is an open source remote procedure call (RPC) system initially developed at Google. **gRPC** is a framework to efficiently connect services and build distributed systems.

It is focused on high performance and uses the **HTTP/2** protocol to transport binary messages. It is relies on the **Protocol Buffers language** to define service contracts. Protocol Buffers, also known as **Protobuf**, allow you to define the interface to be used in service to service communication regardless of the programming language.


## How gRPC works ?

Developed by Google, gRPC uses **Protocol Buffers** (Protobuf) as its Interface Definition Language (IDL) and its underlying message interchange format. It also leverages **HTTP/2** for transport, which brings several benefits over the traditional HTTP/1.1 used by REST APIs.

- **Protocol Buffers (Protobuf)**: Instead of using text-based formats like JSON, gRPC uses Protobuf, a binary serialization format. You define the structure of your data and the service methods in a `.proto` file. This file acts as a contract between the client and server.
    
- **HTTP/2**: gRPC is built on HTTP/2, which enables features like multiplexing (sending multiple requests over a single connection), server push, and header compression. This makes communication more efficient, especially in microservices architectures.


![[prot.webp]]

## read about gRPC methods types : 
- or gRPC life cycles.


## advantages of gRPC

## grpc vs rest

