

## Data Control Language (DCL)

DCL commands are used to **manage user permissions and control access** to the data and database objects. These are typically handled by database administrators.

**`GRANT`**: Used to give specific privileges to users or roles (e.g., SELECT, INSERT, UPDATE, DELETE privileges on a table).
```sql
GRANT SELECT, INSERT ON Products TO sales_user;
GRANT ALL PRIVILEGES ON Products TO admin_role;
```

**`REVOKE`**: Used to remove previously granted privileges from users or roles.
```
REVOKE DELETE ON Products FROM sales_user;
```

