

1. **API**
	- An **API** (Application Programming Interface) is a set of rules and protocols that allows two different software programs to communicate with each other and exchange data.

2. **API Endpoint** : 
	- a specific URL or entry point to a specific function or service of a server.
	- client interest with server using API endpoint.
	- follow HTTP protocols.
3. **REST :** 
	- REST stands for  Representational State Transfer.
	- Architectural style for designing API, primarily used for client server communication over HTTP.
	- They provide set of HTTP methods for communication  : GET, POST
	- They are stateless which means that they do not save states of the request, every request should contain all the required details.




# API design : 

1. **You need to support both mobile app (needs minimal data) and web dashboard (needs full details) from same API. How will you design the API efficiently?**
	1. Using Query Params : 
		1. The client requests for the fields they need via query params eg `fields=name,age,address`
		2. The server returns the specific json. 
		3. simple, cacheable and works for both mobile and web clients.
		4. It does not require to change the API architecture or versioning.
	2. GraphQL : 
		1. Let the client decide the payload.
	3. API Versioning : 
		1. We can have dual endpoints: `/api/v1/mobile/users` and `/api/v1/web/users`.
	4. Backend For Frontend : 
		1. Here, we create separate API gateways per client. 
		2. Mobile BFF aggregates minimal data and web BFF fetches full details. 




# GraphQL



1. What is graphQl
	1. GraphQL is a **query language for APIs**.  The client asks for exactly the data it needs, and the server returns only that data.
	2. REST usually has multiple endpoints like `/users`, `/orders`, `/posts`.  GraphQL usually has **one endpoint** and the client sends a query describing the exact shape of data needed.
2. What problem GraphQl solves ?
	It solves common API problems like:
	- Flexible request : ask what is required
	- **Over-fetching**: getting more data than needed
	- **Under-fetching**: needing multiple API calls to get all data
	- versioning pain in REST
	- frontend/backend coordination issues

3. Main operation in GraphQL :
	There are 3 main operations:
	- **Query**: read data
	- **Mutation**: change data
	- **Subscription**: receive real-time updates



# Sockets 



# GRPC : 

