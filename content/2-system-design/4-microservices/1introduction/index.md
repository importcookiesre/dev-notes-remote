---
title: Introduction to Microservices
date: 2025-07-26
description:
---

Reference : [Atlassian : microservices-architecutre](https://www.atlassian.com/microservices/microservices-architecture/microservices-vs-monolith)

In 2009 Netflix faced growing pains. Its infrastructure couldn’t keep up with the demand for its rapidly growing video streaming services. The company decided to migrate its IT infrastructure from its private data centers to a public cloud and replace its monolithic architecture with a microservices architecture. The only problem was, the term “microservices” didn’t exist and the structure wasn’t well-known.

Netflix became one of the first high-profile companies to successfully migrate from a monolith to a cloud-based microservices architecture.

Netflix has more than a thousand microservices that manage and support separate parts of the platform, while its engineers deploy code frequently, sometimes thousands of times each day.


# 📌 What is Microservices?

**Definition**:  
**Microservices architecture** (or **microservices**) is a software design pattern where an application is **broken down into small, independent services**, each responsible for a **specific business capability**, and each service can be **developed, deployed, and scaled independently**.


| Feature                    | Description                                                                |
| -------------------------- | -------------------------------------------------------------------------- |
| **Independent Services**   | Each microservice is self-contained and handles a specific domain feature. |
| **Decentralized Data**     | Each service may have its **own database**. No shared DB.                  |
| **Technology Agnostic**    | Each service can be written in different languages/tech stacks.            |
| **Independent Deployment** | Services can be deployed without impacting others.                         |
| **Communication**          | Services talk to each other via **APIs (REST/gRPC/Event-driven)**.         |


## ⚙️ How Services Communicate?

| Communication Type | Protocol                  | Use Case                        |
| ------------------ | ------------------------- | ------------------------------- |
| REST API           | HTTP/HTTPS                | Standard CRUD interactions      |
| gRPC               | HTTP/2 + Protocol Buffers | High performance internal calls |
| Message Queue      | Kafka, RabbitMQ           | Event-driven communication      |

## Advantages of microservices:  (by atlassian)

Microservices are by no means a silver bullet, but they solve a number of problems for growing software and companies. Since a microservices architecture consists of units that run independently, each service can be developed, updated, deployed, and scaled without affecting the other services. Software updates can be performed more frequently, with improved reliability, uptime, and performance. Atlassian went from pushing updates once a week, to two to three times a day.


**Agility** – Promote agile ways of working with small teams that deploy frequently.

**Flexible scaling** – If a microservice reaches its load capacity, new instances of that service can rapidly be deployed to the accompanying cluster to help relieve pressure. We are now multi-tenanant and stateless with customers spread across multiple instances. Now we can support much larger instance sizes. 

**Continuous deployment** – We now have frequent and faster release cycles. Before we would push out updates once a week and now we can do so about two to three times a day. 

**Highly maintainable and testable** – Teams can experiment with new features and roll back if something doesn’t work. This makes it easier to update code and accelerates time-to-market for new features. Plus, it is easy to isolate and fix faults and bugs in individual services.

**Independently deployable** – Since microservices are individual units they allow for fast and easy independent deployment of individual features. 

**Technology flexibility** – Microservice architectures allow teams the freedom to select the tools they desire. 

**High reliability** – You can deploy changes for a specific service, without the threat of bringing down the entire application.  
  
**Happier teams** – The Atlassian teams who work with microservices are a lot happier, since they are more autonomous and can build and deploy themselves without waiting weeks for a pull request to be approved.

## Disadvantages : 

When we moved from a small number of monolithic codebases to many more distributed systems and services powering our products, unintended complexity arose. We initially struggled to add new capabilities with the same velocity and confidence as we had done in the past. Microservices can add increased complexity that leads to development sprawl, or rapid and unmanaged growth. It can be challenging to determine how different components relate to each other, who owns a particular software component, or how to avoid interfering with dependent components. 

With Vertigo, we built a common functionality that would power our existing products and future products we acquire and build. If you are a single product company, microservices may not be necessary.

The disadvantages of microservices can include: 

**Development sprawl** – Microservices add more complexity compared to a monolith architecture, since there are more services in more places created by multiple teams. If development sprawl isn’t properly managed, it results in slower development speed and poor operational performance. 

**Exponential infrastructure costs** – Each new microservice can have its own cost for test suite, deployment playbooks, hosting infrastructure, monitoring tools, and more.

**Added organizational overhead** – Teams need to add another level of communication and collaboration to coordinate updates and interfaces. 

**Debugging challenges** – Each microservice has its own set of logs, which makes debugging more complicated. Plus, a single business process can run across multiple machines, further complicating debugging. 

**Lack of standardization** – Without a common platform, there can be a proliferation of languages, logging standards, and monitoring. 

**Lack of clear ownership** – As more services are introduced, so are the number of teams running those services. Over time it becomes difficult to know the available services a team can leverage and who to contact for support.


# Monolith 


A **monolithic architecture** is a single, unified codebase where all the application’s features (Frontend, business logic, data access layer, etc.) are bundled together and deployed as one unit.

### Benefits :

When developing using a monolithic architecture, the primary advantage is fast development speed due to the simplicity of having an application based on one code base.

**Easy deployment** – One executable file or directory makes deployment easier.

**Development** – When an application is built with one code base, it is easier to develop.

**Performance** – In a centralized code base and repository, one API can often perform the same function that numerous APIs perform with microservices.

**Simplified testing** – Since a monolithic application is a single, centralized unit, end-to-end testing can be performed faster than with a distributed application.   
  
**Easy debugging** – With all code located in one place, it’s easier to follow a request and find an issue.

### Drawbacks :

As with the case of Netflix, monolithic applications can be quite effective until they grow too large and scaling becomes a challenge. Making a small change in a single function requires compiling and testing the entire platform, which goes against the agile approach today’s developers favor.

The disadvantages of a monolith include: 

**Slower development speed** – A large, monolithic application makes development more complex and slower.

**Scalability** – You can’t scale individual components.

**Reliability** – If there’s an error in any module, it could affect the entire application’s availability.

**Barrier to technology adoption** – Any changes in the framework or language affects the entire application, making changes often expensive and time-consuming.

**Lack of flexibility** – A monolith is constrained by the technologies already used in the monolith.

**Deployment** – A small change to a monolithic application requires the redeployment of the entire monolith.

