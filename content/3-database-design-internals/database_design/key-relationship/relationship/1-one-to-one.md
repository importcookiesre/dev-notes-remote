

Each row in **Table A** relates to **exactly one row** in **Table B**.
- here user_id attribute in user_profiles ensure that there will be only one user_profile row in the table therefore a one-to-one relationship.
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
✅ A user has exactly one profile.  
💡 Used in: separating **optional details** (profile, settings, addresses) from main entity.

![[Pasted image 20260316065054.png]]