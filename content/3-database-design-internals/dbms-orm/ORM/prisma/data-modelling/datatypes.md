


| Prisma Type          | PostgreSQL Type            | Notes                                |
| -------------------- | -------------------------- | ------------------------------------ |
| `Int`                | `INTEGER` or `INT`         | 32-bit signed                        |
| `BigInt`             | `BIGINT`                   | 64-bit signed                        |
| `Float`              | `DOUBLE PRECISION`         | IEEE 754                             |
| `Decimal`            | `DECIMAL(p, s)`            | Arbitrary precision                  |
| `Boolean`            | `BOOLEAN`                  | `true` or `false`                    |
| `String`             | `TEXT`                     | Or `VARCHAR`, depending on migration |
| `DateTime`           | `TIMESTAMP WITH TIME ZONE` | Timezone-aware                       |
| `Json`               | `JSONB`                    | Preferred over `JSON` for indexing   |
| `Bytes`              | `BYTEA`                    | Binary data                          |
| `Unsupported("...")` | Any custom PostgreSQL type | For unsupported native types         |


### Enum types :

- SUPERMAN = SUPERADMIN
```jsx
enum Role {
  USER
  ADMIN
  SUPERMAN
}

model User {
  id   Int  @id @default(autoincrement())
  role Role @default(USER)
}
```