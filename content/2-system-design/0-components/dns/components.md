

## ttl (time to live) : 

TTL (Time to Live) is ==a value that determines how long a DNS resolver should cache a specific DNS record before it needs to be refreshed==. This caching improves performance by preventing repeated lookups, but it also means updates to a record will not take effect until the old, cached information expires. TTL is set in seconds, with default values often being one hour, and the duration can be a trade-off between faster performance and faster updates. 

How it works

- When your computer first requests a domain name (like `example.com`), the DNS process finds its IP address.
- The intermediate DNS servers and your local resolver cache this IP address along with the TTL value associated with that record.
- For the duration specified by the TTL, your computer will use the cached IP address without having to make a new DNS lookup.
- Once the TTL expires, the resolver requests a new IP address for that domain, effectively getting a new "expiration date" for the updated record.



