


## 1. cli : 


#### 1. `drizzle-kit generate`

- compares the current drizzle schema against your shadow database (or production db if no shadow is configured ) and generate the migration file (.sql)

#### 2. `drizzle-kit introspect`

- Reads the structure of an **existing database** and generates the corresponding Drizzle schema TypeScript files.


#### 3. `drizzle-kit migrate`

**Purpose:** Runs the pending, **unapplied migration SQL files** located in your configured migrations folder (usually `/drizzle`). This updates the structure of your development or production database.

#### 4. `drizzle-kit check`

- **Purpose:** Compares the current Drizzle schema files (.sql) with the state of the database and informs you if a new migration is needed. This is a non-destructive check.

#### 5.`drizzle-kit drop`

- **Purpose:** **Completely drops** (deletes) all tables and data in the configured development database.
Use this carefully, usually only in a **development environment**.

