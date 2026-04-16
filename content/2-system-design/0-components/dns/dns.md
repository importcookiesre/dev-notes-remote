[cloudflare docs](https://www.cloudflare.com/learning/dns/what-is-dns/)


## NOTE: 

- a server can have both IPv4 and IPv6 at the same time. This is called Dual Stack.
	- Dual-stack means that the server's network interface, operating system, and applications are configured to handle both IP protocol stacks concurrently.
	- **IPv4 Address:** The traditional address (e.g., `192.168.1.1`). 32 bits
	- **IPv6 Address:** The newer, much longer address (e.g., `2001:db8::1`). 128 bits
	- and a single sub domain can point to both of these IP.
- If you use a `CNAME` record, then you cannot have any other records for that specific subdomain.
	- **If** you create a **CNAME** for `blog.example.com`, you **cannot** have an **A**, **AAAA**, or even a **TXT** record _for_ `blog.example.com`. The CNAME must be the **only** record for that hostname.

	- _Why?_ The CNAME tells the world, "Look up the address for this other name instead." If you also provided an A record, it would create a conflict and confuse the system.

- **If you use an A or AAAA record, you can have as many other records as you need.**
	- **If** you use an **A** or **AAAA** record for `blog.example.com`, then you **can** add all the other records you need, including:
	    - **TXT** records (as many as you want).
	    - **MX** records (for email).
	    - **SRV** records.



## 1. What is DNS ?

DNS is a distributed, hierarchical, key-value pair database whose take is to translate human understandable domain name to machine usable IP address.

google.com -> `142.250.183.78`
DNS is not a single server and not a database lookup. It is a recursive discovery process (hierarchical) across multiple layers of authority.

It uses : UDP for fast lookup.



## 2. The high level flow :

DNS resolution follow this chain : 
1. Application origin: browser, curl, Go, HTTP client
2. Operating System Resolver
3. Local DNS cache
4. Recursive DNS resolver : ISP / public resolver
5. Root name server
6. TLD name server (.com, .in, .org)
7. Authoritative name server : for the domain
8. IP address returned -> cached -> used for TCP connection.


