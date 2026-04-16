

🔹 **Connection and Database Management**
```
psql -U username -d dbname       # Connect to a database
psql -U postgres                 # Default user (may require password)
\q                               # Quit the psql prompt
\c dbname                        # Connect to a different database
\l                               # List all databases
\dt                              # Display all tables
\dn                              # List all schemas
\dt schema_name.*                # List all schemas under the schema name
\du                              # List all roles/users
\conninfo                        # Prints the connected db
\d employees                     # Show table definition
```

🔹 **Database and Role Commands**
```go
CREATE DATABASE mydb;
DROP DATABASE mydb;
CREATE USER myuser WITH PASSWORD 'mypassword';
DROP USER myuser;
GRANT ALL PRIVILEGES ON DATABASE mydb TO myuser;
```


🔹 **Other Useful Commands**
```
-- Describe a database
\conninfo

-- Execute SQL script from file
\i path/to/script.sql
```