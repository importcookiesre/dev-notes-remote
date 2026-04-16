Postgres has **built-in types for IP + network handling**:

| Type       | Description                                 |
| ---------- | ------------------------------------------- |
| `INET`     | IP address (IPv4/IPv6) with optional subnet |
| `CIDR`     | Network range (strict subnet)               |
| `MACADDR`  | MAC address                                 |
| `MACADDR8` | Extended MAC (EUI-64)                       |

## use cases : 

1. login tracking using IP
2. IP blocking
3. dashboard and analytics



## 1. INET  :

Stores:
- IPv4
- IPv6
- Optional subnet


```sql
CREATE TABLE access_logs (
    ip INET
);

INSERT INTO access_logs (ip)
VALUES 
('192.168.1.10'),
('192.168.1.0/24'),
('2001:db8::1');
```


filtering : 
```sql
SELECT * FROM access_logs
WHERE ip = '192.168.1.10';
```

network containment : 

```sql
SELECT * FROM access_logs
WHERE ip << '192.168.1.0/24';
```
returns all the IP belongs to this subnet
## 2. CIDR : 

- Stores **network blocks only**
- Enforces valid subnet

```sql
CREATE TABLE networks (
    net CIDR
);

INSERT INTO networks VALUES ('192.168.1.0/24');
```


this will give error : 
```sql
INSERT INTO networks VALUES ('192.168.1.10/24');
-- ERROR (not a valid network address)
```

| Feature           | INET | CIDR |
| ----------------- | ---- | ---- |
| Single IP         | ✅    | ❌    |
| Network           | ✅    | ✅    |
| Strict validation | ❌    | ✅    |
|                   |      |      |
|                   |      |      |



## 3. MACADDR  

stores MAC address : 

```sql
CREATE TABLE devices (
    mac MACADDR
);

INSERT INTO devices VALUES ('08:00:2b:01:02:03');
```


## 4. MACADDR8 

