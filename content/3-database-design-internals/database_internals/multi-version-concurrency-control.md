
> [!todo] 
> 1. Read/write lock matrix (lock compatibility matrix)



> [!Question] The Fundamental Principle
> **Readers do not block Writers, and Writers do not block Readers**.

- It is a Transaction management mechanism to achieve high concurrency.



## Problem that MVCC solved :

Before the widespread adoption of Multi-Version Concurrency Control (MVCC), relational databases relied primarily on ==**Strict Two-Phase Locking (2PL)**== using a ==Read/Write Lock== matrix to maintain ACID isolation.