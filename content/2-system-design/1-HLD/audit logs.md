



## 1. What is an audit log ?

An **Audit Log** is a **permanent, chronological record**(arranged in the order in which the events happened)  of important actions taken inside your system.
Think of it like a **black box recorder** for your backend.
It captures:
- **Who** did something
- **What** they did
- **When** they did it
- **What changed** (before vs after)
- **Where** the action originated
- **Why** it happened (optional metadata)

It is **append-only** (never overwritten), which makes it ideal for tracking and compliance.

Audit Logs are **essential** for any serious backend system because they provide:

- Debugging
- Security
- Compliance
- Accountability
- Rollback
- Anti-abuse
- Analytics

```python
AuditLog {
  id: ObjectId
  actorId: string | null           // user/admin/system
  resourceType: string             // "User" | "SubDomain" | "DNSRecord" | ...
  resourceId: string               // ID in its original table
  action: string                   // create | update | delete | login | etc.
  oldValue: any | null
  newValue: any | null
  meta: any | null                 // IP, user-agent, request ID, API key
  createdAt: Date
}

```


## if i have 5 tables then do i need to create 5 different audit log table ?

**You create ONE central `audit_logs` table for the entire system — not one per entity.**
Audit logs are meant to be:

- append-only
- universal
- queryable across the entire system
- easy to search & filter
- simple to integrate with monitoring tools

