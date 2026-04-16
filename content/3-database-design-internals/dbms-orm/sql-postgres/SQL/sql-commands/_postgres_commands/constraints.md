



## Constraints:

| Constraint    | Description                                                                                                       |
| ------------- | ----------------------------------------------------------------------------------------------------------------- |
| `NOT NULL`    | Ensures a column cannot be null.                                                                                  |
| `PRIMARY KEY` | Uniquely identifies each row; combines `NOT NULL` and `UNIQUE`.                                                   |
| `FOREIGN KEY` | Ensures data integrity by referencing another table.                                                              |
| `UNIQUE`      | Ensures all values in a column are distinct.                                                                      |
| `CHECK`       | Validates data according to a condition.                                                                          |
| `DEFAULT`     | Provides a default value if none is given.                                                                        |
| `EXCLUSION`   | Ensures no two rows meet a specific condition combination.                                                        |
| `INDEX`       | Optimizes data retrieval by indexing columns. - Automatically created for `PRIMARY KEY` and `UNIQUE` constraints. |


**check**
```sql
CREATE TABLE employees (
  id SERIAL PRIMARY KEY,
  age INT CHECK (age >= 18)
);
```

**example**
```
CREATE TABLE employees (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(100) UNIQUE,
  age INT CHECK (age >= 18),
  department_id INT,
  FOREIGN KEY (department_id) REFERENCES departments(id)
);
```



#### **adding constraints**

**Add a `UNIQUE` constraint**:

```sql
ALTER TABLE employees
ADD CONSTRAINT unique_email UNIQUE (email);
```

**Add a `PRIMARY KEY` constraint**

```sql
ALTER TABLE orders
ADD CONSTRAINT pk_order PRIMARY KEY (order_id);
```

**Add a `FOREIGN KEY` constraint**:

```sql
ALTER TABLE orders
ADD CONSTRAINT fk_customer FOREIGN KEY (customer_id) REFERENCES customers(id);
```

**Add a `CHECK` constraint**:

```sql
ALTER TABLE products
ADD CONSTRAINT check_price CHECK (price > 0);
```

#### droping constraints : 

To remove a constraint, you need to know its name. You can use the `DROP CONSTRAINT` clause.
```sql
ALTER TABLE employees
DROP CONSTRAINT unique_email;
```

#### Important Note on Altering Constraints 🔄

For many types of constraints (like `PRIMARY KEY`, `FOREIGN KEY`, and `UNIQUE`), you can't directly "alter" them to change their definition. Instead, the common practice is to **drop the old constraint and then add a new one** with the updated definition. For instance, to change a foreign key's `ON DELETE` behavior, you would drop the existing foreign key and re-add it with the new rule.
