



#### contraints and attributes : 

| Attribute / Constraint | Description                                                                  | Example Usage                                                 |
| ---------------------- | ---------------------------------------------------------------------------- | ------------------------------------------------------------- |
| `@id`                  | Marks a field as the **primary key**.                                        | `id Int @id`                                                  |
| `@default()`           | Sets a **default value** for a field.                                        | `createdAt DateTime @default(now())`                          |
| `@unique`              | Ensures the field has **unique values**.                                     | `email String @unique`                                        |
| `@relation()`          | Defines **relations** between models.                                        | `author User @relation(fields: [authorId], references: [id])` |
| `@updatedAt`           | Automatically **updates timestamp** when a record is modified.               | `updatedAt DateTime @updatedAt`                               |
| `@map()`               | Maps a field or model name to a different name in the **database**.          | `firstName String @map("first_name")`                         |
| `@ignore`              | Tells Prisma to **ignore this field or model** in schema generation.         | `legacyField String @ignore`                                  |
| `@@unique()`           | Creates a **compound unique constraint** across multiple fields.             | `@@unique([firstName, lastName])`                             |
| `@@id()`               | Creates a **composite primary key**.                                         | `@@id([userId, postId])`                                      |
| `@@map()`              | Maps the model/table name in the Prisma schema to a different database name. | `@@map("user_table")`                                         |
| `@db.*`                | Specifies a **database-specific native type**.                               | `price Decimal @db.Decimal(10, 2)`                            |



| Function / Constant | Description                                                            | Example Usage                          |
| ------------------- | ---------------------------------------------------------------------- | -------------------------------------- |
| `now()`             | Sets the current timestamp as the default value.                       | `createdAt DateTime @default(now())`   |
| `autoincrement()`   | Automatically increments a numeric field (usually for IDs).            | `id Int @id @default(autoincrement())` |
| `uuid()`            | Generates a UUID string as the default value.                          | `id String @id @default(uuid())`       |
| `cuid()`            | Generates a collision-resistant ID string (shorter than UUID).         | `id String @id @default(cuid())`       |
| `true` / `false`    | Boolean constants for default values.                                  | `published Boolean @default(false)`    |
| `"string"`          | String literal default value.                                          | `role String @default("USER")`         |
| `1`, `0`, etc.      | Numeric literal default value.                                         | `views Int @default(0)`                |
| `@updatedAt`        | Auto-updates timestamp on every update. _(not used with `@default()`)_ | `updatedAt DateTime @updatedAt`        |