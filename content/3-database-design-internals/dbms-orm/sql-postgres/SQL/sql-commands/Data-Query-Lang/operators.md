



## Operators : 

### **1. Comparison Operators**

| Operator     | Description              | Example           |
| ------------ | ------------------------ | ----------------- |
| `=`          | Equal to                 | `salary = 50000`  |
| `!=` or `<>` | Not equal to             | `salary <> 50000` |
| `>`          | Greater than             | `age > 30`        |
| `<`          | Less than                | `age < 30`        |
| `>=`         | Greater than or equal to | `age >= 30`       |
| `<=`         | Less than or equal to    | `age <= 30`       |

### **2. Logical Operators**

| Operator | Description                         | Example                             |
| -------- | ----------------------------------- | ----------------------------------- |
| `AND`    | All conditions must be true         | `age > 30 AND department = 'Sales'` |
| `OR`     | At least one condition must be true | `age > 30 OR department = 'Sales'`  |
| `NOT`    | Reverses a condition                | `NOT department = 'HR'`             |

### **3. Pattern Matching Operators**

| Operator   | Description             | Example                                    |
| ---------- | ----------------------- | ------------------------------------------ |
| `LIKE`     | Matches a pattern       | `name LIKE 'J%'` (Names starting with 'J') |
| `NOT LIKE` | Doesn't match a pattern | `name NOT LIKE 'J%'`                       |

**Wildcards used with `LIKE`:**

- `%` → Matches any number of characters (`'J%'` → Starts with J)
- `_` → Matches a single character (`'J_n'` → Matches 'Jan', 'Jon', etc.)

### **4. Set Operators**

|Operator|Description|Example|
|---|---|---|
|`IN`|Matches any value in a list|`department IN ('HR', 'Finance', 'IT')`|
|`NOT IN`|Excludes values in a list|`department NOT IN ('HR', 'Finance')`|

### **5. Range Operators**

|Operator|Description|Example|
|---|---|---|
|`BETWEEN`|Falls within a range (inclusive)|`salary BETWEEN 40000 AND 60000`|
|`NOT BETWEEN`|Falls outside a range|`salary NOT BETWEEN 40000 AND 60000`|

### **6. NULL Operators**

|Operator|Description|Example|
|---|---|---|
|`IS NULL`|Checks for NULL values|`manager_id IS NULL`|
|`IS NOT NULL`|Checks for non-NULL values|`manager_id IS NOT NULL`|


