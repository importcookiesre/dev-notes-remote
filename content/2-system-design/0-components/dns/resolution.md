It is a solid channel for learning, feedback, and early credibility, but dangerous if it becomes your only growth lever.
high level flow : 

```python
Browser
 ↓
OS DNS Resolver (stub resolver)
 ↓
Recursive Resolver (ISP / Public DNS)
 ↓
Root DNS Server
 ↓
TLD DNS Server (.com)
 ↓
Authoritative DNS Server (example.com)
 ↓
IP address returned

```

## 3. DNS resolution : 

### The high level flow :

DNS resolution follow this chain : 
1. Application origin: browser, curl, Go, HTTP client
2. Operating System Resolver
3. Local DNS cache
4. Recursive DNS resolver : ISP / public resolver
5. Root name server
6. TLD name server (.com, .in, .org)
7. Authoritative name server : for the domain
8. IP address returned -> cached -> used for TCP connection.




### step 1. The application asks for a domain :

You type : `https://api.example.com`
Your browser does **not** know what an IP is for this domain.
So it asks the operating system:

> “Resolve `api.example.com` for me.”


The application uses an OS APIs:
- Linux: `getaddrinfo()`
- macOS: `mDNSResponder`
- Windows: DNS Client Service

==At this point, no network happens yet==

### step 2. OS resolver checks local sources :

Before touching the network, the OS checks local resolution sources:
#### 1. `/etc/hosts`
If you have :
```bash
127.0.0.1 api.example.com
```
Resolution stops immediately. `/etc/hosts` overrides DNS.


#### 2. OS DNS cache : 

The OS keeps a cache of recently resolved domains.
If found and TTL (time-to-live) hasn’t expired:
- Resolution stops
- IP is returned instantly
Only if both fail does the OS go to the network.

### Step 3. Query sent to a recursive DNS resolver

OS now sends DNS query to recursive resolver.
This resolver is usually :
- Your ISP's DNS server
- OR a public resolver (8.8.8.8, 1.1.1.1)

Your machine never talks to the authoritive servers directly, it just deligates the DNS query to recursive resolver and waits for the result.
This query is typically send using :
- UDP port 53 : fast, stateless
- TCP port 53 : large responses or retiers , less commonly


### Step 4. Recursive resolver checks its own cache : 

Now though your machine has deligate the query to the DNS recursive resolver, recursive resolver check for the IP in its own cache.
If the resolver already has the IP and TTL is valid, it immediately replies and DNS resolution ends here. `api.example.com -> IP`
If it is not present in the cache then it initiate **recursive discovery**.


### Step 5. Root name server : The first in the hierarchy

The resolver asks a **root name server** :
> Who knows about `.com` ?

Root Name Server know the IP addresses of the TLD servers (eg. `.com` , `.in` )
![[Pasted image 20251222122234.png]]

### Step 6. TLD name servers (.com, .org etc)

TLD stands for Top Level Domain. Eg : .com, .in. 
They are also servers and have an IP address
The resolver now ask `.com` :
`Who is authoritative for example.com`

TLD servers responds :
```python
example.com is handled by:
ns1.example-dns.com
ns2.example-dns.com
```
Still we don't have an IP.
Now the request goes to Authoritative name server.

### Step 7. Authoritative name server : The source of truth

Now the resolver asks the authoritative name server

What is the IP for `api.example.com`?

This server hosts the domain’s DNS zone file and responds with records like:
- A record → IPv4
- AAAA record → IPv6
- CNAME → alias
- MX → mail server
- TXT → verification, SPF, etc.

```python
api.example.com → 203.0.113.42
TTL: 300 seconds
```

### Step 8. Response travels back and caches everywhere

The IP flows back down the chain:

1. Authoritative → Recursive resolver
2. Recursive resolver caches it (for TTL duration)
3. OS receives response
4. OS caches it
5. Application receives IP
==Now DNS resolution is complete.==

### Step 9: DNS is done - networking begins

1. TCP 3 way handshake happens (or QUIC for HTTP/3)
2. TLS handshake (for HTTPS)
3. HTTP request sent
4. Server responds

