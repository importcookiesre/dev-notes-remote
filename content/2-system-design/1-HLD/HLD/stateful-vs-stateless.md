

## Definition :

**What are states ?**
	States represents data, context, or session information stored between client-server interactions.

**What is sessions ?** 
	A session is basically ==a way to keep track of the user's state across multiple requests.== HTTP is stateless so the user need to send states on every request. When the user logs in, the server creates a session containing relevant data (like role, user_id etc) which is stored on the server. The session identifier is given to the user generally passed in cookies or headers. On each request it is used to identify the user after validation the session with server data. 
	Basically it is used to identify a user between multiple request in a stateless network communication.
### Stateful

In a stateful design, the server actively stores information about the client's session in its memory or local storage.

Examples:
- Login session stored on server
- **Real-Time Chat Applications:** WebSockets keeping a continuous, open connection to push and pull messages instantly.
- **Multiplayer Gaming:** Servers tracking the exact real-time coordinates of multiple players in a shared world.
- **Complex Multi-Step Transactions:** A checkout wizard where the server temporarily remembers the items in step 1 while the user navigates step 2.
- **Streaming Services:** Maintaining an active buffer and tracking exactly where a user is in a video feed.

### Stateless

A system is stateless when each request contains all information needed to process it. 


Examples:
- **RESTful APIs:** Fetching a list of blog posts or submitting a form. The server processes the request, sends a response, and forgets the transaction.
- **Serverless Environments:** NextJS API routes or AWS Lambda functions spin up, execute a specific task, and shut down, meaning they inherently cannot hold state in memory.
- **Token-Based Authentication:** Using JWTs in an Express middleware where the token itself holds the user's identity data, completely freeing the server from tracking "who is logged in" in its memory.
- **Content Delivery (CDNs):** Serving static assets where every request is entirely independent of previous ones.



# Stateless architecture : 

Stateless architecture eliminates the requirment of 
- storing session in DB
- verifying it against user input data
- managing states in database ie deleting the expired sessions.
It can be implemented in a problem statement where a session is stored in the DB. 





## Implementation : 


### 1. JSON Web Tokens : JWT


### 2. HMAC Signed Strings


### 3. Asymmetric Cryptography




## Use cases : 

1. **Microservices communication :** service to service Auth
	In a monolithic application, checking a session ID against a centralized Postgres or Redis database is fast. But in a distributed system, you might have an API Gateway, a User Service, an Order Service, and a Payment Service.
	
	- **The Stateful Problem:** If every service needs to validate the user's session, they all have to make a network call to a centralized Session Service or Redis cluster. This creates a massive bottleneck and a single point of failure.
    
	- **The Stateless Solution:** The API Gateway authenticates the user once and issues a JWT. This JWT is passed downstream in the `Authorization` header to the Order Service and Payment Service. Because the JWT contains the `user_id` and role (`admin` or `customer`), those downstream Go services can independently verify the signature using a shared public key and trust the payload instantly—zero database lookups.

 2. **Password Reset Links**

	- **The Stateful Problem:** You create a `password_resets` table in Postgres. You have to handle race conditions if the user clicks "Forgot Password" three times in a row (do you invalidate the old ones? keep them all?).
	    
	- **The Stateless Solution:** You generate a JWT with a short expiration (e.g., 15 minutes), the `user_id`, and perhaps a hash of their _current_ password. If they use the link, the backend verifies the JWT. If the password hash in the db matches the one in the JWT payload, the link is valid. If they already reset their password, the database hash changed, and the old JWT automatically becomes invalid without you needing to store any state.

3. **Pre-Signed URLs**

	The Backend verifies the user, and then dynamically generates a "Pre-signed URL" (which operates heavily on stateless cryptographic signing principles, much like a JWT). 
	The user's browser uses that URL to download the file _directly_ from the CDN/S3.


# Problems You Must Handle in Stateless Session Management

![[Pasted image 20260405113058.png]]
Stateless session management is attractive because it reduces server-side storage and makes scaling simpler. The server does not need to remember every active session, which keeps authentication fast and lightweight.

But that simplicity comes with trade-offs.

Once you move authentication state out of the server and into a signed token, you inherit a new set of problems that you must handle carefully. A stateless system is not automatically safer or better. It is simply a different set of engineering choices.

## 1. Token revocation

A signed token remains valid until it expires. That means if a user logs out, changes their password, or their token is stolen, the server cannot easily invalidate that token unless some server-side state is introduced.
This is one of the biggest weaknesses of pure stateless auth.
Common approaches to reduce the impact:

- Keep access tokens very short-lived
- Use refresh tokens for re-authentication
- Track a `session_version` or token version in the database for forced logout.
## 2. Stolen token risk

If an attacker copies a valid token, they can impersonate the user until the token expires.
Because the server trusts the signature, not the physical device, possession of the token is enough.

Common defenses include:

- Short token expiry
- Secure client-side storage
- HTTPS only
- Device binding or token fingerprinting in sensitive systems
- Refresh token rotation

## 3. Logout is not truly instant

In a pure stateless system, logout usually means the client deletes the token. The server does not necessarily know that the token should no longer be accepted.

So the token often remains usable until its expiry time.

That is why many real-world systems use a hybrid design:

- Access token: stateless and short-lived
- Refresh token: stateful and revocable

This gives you performance without giving up control completely.

## 4. The data staleness problem

A token can carry claims that become outdated.

For example, a user may have `"role": "admin"` in their token, but later that role may be removed in the database. If the token is still valid, the server may continue to trust stale authorization data.

This creates a dangerous gap between reality in the database and what the token says.

Practical fixes:

- Force a refresh when sensitive data changes
- Use background token replacement in the frontend
- Keep critical authorization checks stateful for high-risk actions

## 5. Token size

A token should stay small.

If you store too much information inside it, the token becomes:

- large
- expensive to send on every request
- stale when user data changes

A token should carry only the minimum necessary claims. It is not a replacement for your database.

## 6. Sensitive data leakage

Tokens are usually encoded, not encrypted. That means anyone who gets the token can read its payload.

For that reason, never put sensitive information inside a token, such as:

- passwords
- OTPs
- private profile data
- secrets
- payment data

A token should be treated as visible metadata, not a safe vault.

## Final thought

Stateless session management is powerful, but it shifts the burden from server memory to token design, expiry strategy, revocation strategy, and authorization checks.

The real skill is not choosing stateless auth blindly. It is knowing where stateless works well, and where you must introduce a stateful control layer to stay secure.