
[[PLAYGROUND2]]

## What are Relationships in DBMS?

- A **relationship** defines how two or more tables are logically connected.
- In **ER Model** we draw relationships with diamonds, but in PostgreSQL we implement them using **foreign keys**.
	- The child always contains the foreign key which references to the primary key of the parent table.

- why we need a relationship ?
	- If there are user and credit card details, here we have two ways to store data : 
		- Keep all the data in a table : user & credit card table : 
			- Problems : 
				- overfetching.
				- Breaking single responsiblity principle.
				- Difficult to have multiple card associated with the User.
		- Have seperate table for User and Credit Card:
			- Use a one to one relationship between user and card.
			- Now we can have many credit card associated with the user.
			- 
## Logical flow of the relationship : 

- child or dependent table points to the parent table (Independent table).
- This helps to understand why users table is not having a user_profile_id field that points to user_profile.
```sql
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL
);

CREATE TABLE user_profiles (
    profile_id SERIAL PRIMARY KEY,
    user_id INT UNIQUE,  -- ensures 1-to-1
    bio TEXT,
    FOREIGN KEY (user_id) REFERENCES users(user_id) ON DELETE CASCADE
);
```

### 1. Identifying the Parent and Child

In any relational database relationship, one table is typically the **parent** (the source or independent entity), and the other is the **child** (the dependent entity).

- **Parent Table: `users`**
    - This is the **core, standalone entity**. A user can exist even if they haven't created a profile yet. It's the primary subject of the application.

- **Child Table: `user_profiles`**
    - This is the **dependent, supplementary entity**. A profile cannot exist without a user. It exists only to provide _additional detail_ about an existing user.
### 2. The Direction of the Foreign Key

The fundamental rule of foreign keys is that they must point from the **dependent table (Child)** to the **independent table (Parent)**. The foreign key is the mechanism that enforces this dependency.

- **Correct Flow:** The user_profiles table must contain the user_id because every row in user_profiles **depends on** and **must reference** a valid, existing row in the users table.
    
    - **Analogy:** Think of a **Book** and its **ISBN**. A table of _Book Reviews_ (the child) must contain the _Book's ISBN_ (the foreign key) to specify which book it belongs to. You would never put a _Review ID_ in the main _Book_ table.


### 3. If I create a profile attribute in users, then it will be easy to query as by just fetching the users I will have access to user's profile also.

```sql
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    profile_id INT UNIQUE, -- Foreign Key to user_profiles
    FOREIGN KEY (profile_id) REFERENCES user_profiles(profile_id)
);
-- ... and user_profiles would not have the FK
```

```sql
-- You would STILL need a JOIN to get the 'bio' text,
-- as only profile_id is in the users table.
SELECT
    u.username,
    up.bio
FROM
    users u
LEFT JOIN
    user_profiles up ON u.profile_id = up.profile_id;
```

❌ **Cons:** This approach offers no real querying advantage and is worse for performance because the users table is cluttered with profile_id values, many of which are NULL, making the most frequently used table larger and slower.

Standard (Correct) Approach: Requires a JOIN

```sql
SELECT
    u.username,
    up.bio
FROM
    users u
LEFT JOIN
    user_profiles up ON u.user_id = up.user_id;
```
✅ **Pros:** users table is lean and fast. NULL values only appear in the results, not in the core table.






## Types of relationship

There are three types of relationship : 
1. one-to-one  1:1
2. one-to-many 1:N
3. many-to-many M:N




### 2. one-to-many 1:N


### 3. many-to-many relationship M:N
