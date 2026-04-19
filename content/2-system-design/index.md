

## first princple :

Frist principle thinking means breaking a problem down to its most fundamental blocks and then solving upward from those basics by reasoning instead of relying on assumptions, conventions or existing solutions.

Normal thinking:
> “Rate limiters are usually implemented with Redis.”

First-principles thinking:

- What is the core problem? → **Limit how many requests happen in a time window.**
- What do I fundamentally need?
    - A **counter**
    - A **time window**
    - A **way to reject requests when the limit is exceeded**

Now you can build it with:
- memory
- Redis
- a distributed counter
- token bucket algorithm


## 1. How to design Systems ?

