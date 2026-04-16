

# Microservices Synchronous Communication and Practices


As we said before, In **synchronous communication**, the client sends a request with using http protocols and waits for a response from the service. The synchronous communication protocols can be **HTTP** or **HTTPS**

![[w.webp]]

But how we can design and exposing **APIs** with **HTTP** protocols for our microservices ? we should focus on that point.

When we are using a synchronous **request/response-based** communication type, HTTP protocols and REST approaches are the most common way to use to design APIs, especially if we’re exposing APIs to the outside of the microservice cluster.

If we’re communicating between services internally within our microservices cluster, we might also use **binary format communication mechanisms** like gRPC. gRPC is one of the best way to communicate for internal microservice communication, we will see **gRPC** in the upcoming sections.



# E-Commerce Service Communications



![[ecommer.webp]]

As you can see that now these lines representing sync communication. And these communication will be **HTTP based RESTful APIs** which will return to **JSON** objects. But if there is required to communicate internal microservices, its good to choose **gRPC binary protocols** in order to be fastest as posible.

We use different protocols for client call and the internal communication even both type is sync communication. Because client request is good to be REST in order to see payloads explicitly, But **backend communication** can be sacrifice to see payloads instead of pick velocity of response time. **gRPC** much faster than rest.

So we can say that, if we prefer to communicate with synchronous communication, we have several options those are; **HTTP protocols** and **REST approaches**. **gRPC** binary format communications.



# Designing HTTP based RESTful APIs for Microservices

![[3.webp]]
