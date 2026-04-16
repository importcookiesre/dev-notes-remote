

## 1. Message Versioning : 

**The Concept:** In a microservices architecture, the Producer and Consumer are deployed independently. You _cannot_ upgrade both at the exact same millisecond.

- **Scenario:** You change the "User Signup" event structure.
- **Risk:** If you deploy the Producer first, the old Consumer crashes (can't parse new format). If you deploy Consumer first, it might crash reading the old format.

**Strategy A: Additive Changes Only (The Golden Rule)** Follow **Postel's Law**: _"Be conservative in what you do, be liberal in what you accept from others."_

- **Rule:** You can ADD fields, but you can never DELETE or RENAME fields that are currently in use.
    
- **Example:**
    
    - _Version 1:_ `{ "address": "123 Main St" }`
        
    - _Requirement:_ You need to split street and city.
        
    - _Bad (Breaking):_ `{ "street": "123 Main St", "city": "NY" }` (Old consumer looks for "address", gets `null`, crashes).
        
    - _Good (Additive):_ `{ "address": "123 Main St", "street": "123 Main St", "city": "NY" }`
        
- **Deprecation Phase:** Once _all_ consumers are updated to use `street/city`, you can remove `address` in a future release. This is the **Expand-Contract pattern**.

**Strategy B: Versioning in Headers** Explicitly tell the consumer which contract to use.

- **Header:** `X-Message-Version: 2.0`
- **Consumer Logic:**

**Strategy B: Versioning in Headers** Explicitly tell the consumer which contract to use.

- **Header:** `X-Message-Version: 2.0`
    
- **Consumer Logic:**
    
```go
switch msg.Header["Version"] {
case "1.0":
	handleV1(body)
case "2.0":
	handleV2(body)
default:
	// Dead Letter Queue this unknown version
}
```
    
- **Pros:** Clean separation of code.
- **Cons:** You have to maintain legacy code paths (V1) for a long time.
    

**Strategy C: The Dual-Publish (Migration Bridge)** For massive breaking changes where additive isn't enough.

1. Producer sends message to `Exchange_V1` (Old format) **AND** `Exchange_V2` (New format).
2. Old Consumers keep reading V1.
3. New Consumers read V2.
4. Once Old Consumers are retired, stop publishing to `Exchange_V1`.

