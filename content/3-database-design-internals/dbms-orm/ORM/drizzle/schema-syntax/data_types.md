




## 1. Enum : 

#### Using pgEnum: 

1. Define the enum : 
```ts
import { pgEnum } from 'drizzle-orm/pg-core';

// 1. Define the enum object
export const userRoleEnum = pgEnum('user_role', ['admin', 'editor', 'viewer']);
// This generates: CREATE TYPE "user_role" AS ENUM('admin', 'editor', 'viewer');
```

2. use the enum : 
```ts
import { pgTable, serial } from 'drizzle-orm/pg-core';
import { userRoleEnum } from './your-schema-file'; // Import the defined enum

export const users = pgTable('users', {
  id: serial('id').primaryKey(),
  
  // 2. Use the defined enum as a column type
  role: userRoleEnum('role').notNull().default('viewer'),
});
// The 'role' column will have the PostgreSQL type "user_role"
```

3. extract the typescript type : 

```ts
// This extracts the union type: 'admin' | 'editor' | 'viewer'
export type UserRole = (typeof userRoleEnum.enumValues)[number];
```



## 2. dfg