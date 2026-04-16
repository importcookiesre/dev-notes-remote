## 3. Drop 

- Permanently removes an object.
- Use `IF EXISTS` to avoid errors.
- In case of droping a table, if it is associated with any other table with a foreign key then it will not allow to drop it.
	- In this case either :
		- drop the foreign key contraint and then remove it
		- use `drop table users cascade;` - this will not delete the associated tables
```sql
DROP TABLE users;
DROP INDEX idx_users_username;
DROP SCHEMA analytics CASCADE;  -- CASCADE removes dependent objects

DROP TABLE IF EXISTS users;
```



## 4. TRUNCATE

- Deletes all rows **very fast** without scanning.
- Faster than `DELETE FROM users;`
- Cannot be rolled back if executed outside a transaction.
```sql
TRUNCATE TABLE users RESTART IDENTITY; -- reset serial values too
```
