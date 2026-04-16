

## 1. Fundamentals :

- Producers / Publishers :
- Broker
- Consumers
- Decoupling
- message

### 1. Producers / Publishers : 

senders

### 2. Consumers :

receivers

### 3. Decoupling : 




### Message Broker : 

A message broker is ==middleware that acts as an intermediary for applications to communicate, acting as a central hub to receive, store, route, and deliver messages between producers (senders) and consumers (receivers)==, enabling [decoupling](https://www.google.com/search?q=decoupling&oq=message+broker&gs_lcrp=EgZjaHJvbWUyCQgAEEUYORiABDIHCAEQABiABDIHCAIQABiABDIHCAMQABiABDIHCAQQABiABDIHCAUQABiABDIHCAYQABiABDIHCAcQABiABDIHCAgQABiABDIHCAkQABiABNIBCTEwNjAxajBqMagCALACAA&sourceid=chrome&ie=UTF-8&ved=2ahUKEwjWl7i1lMGRAxWLR2wGHaPOMYgQgK4QegYIAQgAEAY) and scalability in distributed systems by handling complex tasks like protocol translation and ensuring reliable message delivery, even if systems are offline. Key examples include RabbitMQ, Kafka, and Amazon SQS, supporting patterns like pub/sub (topic-based) and point-to-point (queue-based).


- **Decoupling:** Producers and consumers don't need to know each other's location or status.
- **Reliability:** Stores messages until consumed, preventing data loss.
- **Asynchronous Communication:** Allows systems to work independently without waiting for immediate responses.
- **Protocol Translation:** Converts messages between different messaging protocols (e.g., MQTT to HTTP).
- **Routing & Filtering:** Directs messages based on rules, topics, or content.

