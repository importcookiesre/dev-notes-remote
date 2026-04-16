


- Modify the structure of the existing table.

```sql
# Add / remove column 
ALTER TABLE users ADD COLUMN last_login TIMESTAMP;
ALTER TABLE users DROP COLUMN last_login;


-- change type
ALTER TABLE users ALTER COLUMN username TYPE TEXT;

-- Modify a column's data type or constraint
ALTER TABLE Products ALTER COLUMN ProductName TYPE VARCHAR(500);

-- rename
ALTER TABLE OldTableName RENAME TO NewTableName;
ALTER TABLE Products RENAME COLUMN Price TO UnitPrice;

-- Drop a column
ALTER TABLE Products
DROP COLUMN StockQuantity;

-- Add a foreign key constraint
ALTER TABLE Products ADD CONSTRAINT FK_Supplier FOREIGN KEY (SupplierID) REFERENCES Suppliers (SupplierID);
```
postgreSQL is powerful here: you can even `ALTER TYPE`, `ALTER DATABASE`, `ALTER ROLE`, etc.



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
