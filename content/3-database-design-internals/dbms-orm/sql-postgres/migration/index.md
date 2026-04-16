

## 🧠 What is a Database Migration?

A **database migration** is a structured way to evolve your database schema over time. It helps track, apply, and reverse changes to the database using version-controlled SQL files.
Think of it like Git for your database schema.
- The migration version is stored in the database to track.

- If any of the migration failed then it locks the migration

| Concept           | Description                                                                  |
| ----------------- | ---------------------------------------------------------------------------- |
| `.up.sql` file    | Contains the SQL statements to apply a change (e.g., create a table).        |
| `.down.sql` file  | Contains the SQL to reverse that change (e.g., drop the table).              |
| Migration version | Each migration is numbered (e.g., `0001`, `0002`, etc.) for order tracking.  |
| Schema table      | A special table in the DB (like `schema_migrations`) keeps track of version. |



## 🔁 How Migrations Work (Step-by-Step)

### ✅ 1. Initial State

Let's say we have a migration file:

```go
0001_create_users_table.up.sql 
0001_create_users_table.down.sql
```

### ✅ 2. `Up()` Migration

- When we run `m.Up()`:
    - It checks the current version (e.g., version 0).
    - Applies `0001_create_users_table.up.sql`.
    - Updates the schema version to 1 in the database.
    - Continues with next migrations (0002, 0003...) if available.

### 3. `Down()` Migration

- When we run `m.Down()`:
    - It checks the current version (e.g., version 3).
    - Applies `0003_description_field.down.sql`.
    - Downgrades schema version to 2.
    - You can also run `.Steps(n)` to go down/up `n` versions.

---
