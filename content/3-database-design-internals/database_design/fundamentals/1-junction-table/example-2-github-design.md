


# hosting platform : Users, repositories, collaborations : 

NOTE: Storing the owner_id in the repo is a bad better, it should be stored in the collaboration table (junction table) as Ownership is a **relationship property**, not an organization property. (not followed here)
- If there is a junction table then all relationship properties belong there.

## 1. Problem statement :

You are building the database for a **Git hosting platform** similar to GitHub.
The platform allows users to create repositories and collaborate with others.
Your job is to design the database schema and answer various analytical queries.

### Core Entities : 
#### 1. Users
People who use the platform.
	suraj
	rohit
Each user can:
• create repositories  
• collaborate on other repositories

#### 2. Repositories
Projects hosted on the platform.

Examples
	go-auth-service  
	distributed-cache  
	postgres-learning  
	os-kernel-notes
Each repository:
• has **exactly one owner**  
• can have **many collaborators**


#### 3. Repository Collaborators

Users can collaborate on repositories.
Important rules:
1. A repository can have **many collaborators**
2. A user can collaborate on **many repositories**
This creates a **many-to-many relationship**.

Each collaborators has a role and created at timestamp : 
```go
maintainer
contributor
reviewer
```


example :
```go
Aman → postgres-learning → maintainer
Riya → postgres-learning → contributor
```

## 2. Rules : 

1. Each repo must have exactly one owner.
2. A user cannot collaborate on the same repo twice.
3. if a user is deleted :
	1. their collaborations must be removed
4. If a repo is deleted :
	 	1. its collaborations must be removed

## Solution : 

### 1. create table :

User
```sql
CREATE TABLE users (
    id uuid PRIMARY KEY DEFAULT uuidv7(),
    username VARCHAR(50) NOT NULL UNIQUE,
    created_at timestamptz DEFAULT now()
);
```

Repository : 
```sql
CREATE TABLE repositories (
    id uuid PRIMARY KEY DEFAULT uuidv7(),
    name VARCHAR(100) NOT NULL,
    owner_id uuid REFERENCES users(id) ON DELETE CASCADE,
    created_at timestamptz DEFAULT now()
);
```

collaborations 
```sql
CREATE TABLE collaborations (
    id uuid PRIMARY KEY DEFAULT uuidv7(),
    user_id uuid REFERENCES users(id) ON DELETE CASCADE NOT NULL,
    repo_id uuid REFERENCES repositories(id) ON DELETE CASCADE NOT NULL,
    role TEXT CHECK (role IN ('maintainer','contributor','reviewer')),
    created_at timestamptz DEFAULT now(),
    UNIQUE(user_id, repo_id)
);
```


### 2. Insert Values ;

```sql 
INSERT INTO users (username) VALUES
('Suraj'),
('Aman'),
('Riya'),
('Neha'),
('Rahul');

-- Repository table :
INSERT INTO repositories (name, owner_id)
VALUES
(
 'postgres-learning',
 (SELECT id FROM users WHERE username='Suraj')
),
(
 'distributed-cache',
 (SELECT id FROM users WHERE username='Aman')
),
(
 'os-kernel-notes',
 (SELECT id FROM users WHERE username='Riya')
),
(
 'go-auth-service',
 (SELECT id FROM users WHERE username='Suraj')
);


-- collaboration table :

INSERT INTO collaborations (user_id, repo_id, role)
VALUES
(
 (SELECT id FROM users WHERE username='Aman'),
 (SELECT id FROM repositories WHERE name='postgres-learning'),
 'maintainer'
),
(
 (SELECT id FROM users WHERE username='Riya'),
 (SELECT id FROM repositories WHERE name='postgres-learning'),
 'contributor'
),
(
 (SELECT id FROM users WHERE username='Neha'),
 (SELECT id FROM repositories WHERE name='distributed-cache'),
 'contributor'
),
(
 (SELECT id FROM users WHERE username='Rahul'),
 (SELECT id FROM repositories WHERE name='os-kernel-notes'),
 'reviewer'
),
(
 (SELECT id FROM users WHERE username='Neha'),
 (SELECT id FROM repositories WHERE name='go-auth-service'),
 'contributor'
); 
```



### Queries : 

### 1. List all repositories with their owners : 

```sql 
SELECT
    r.name AS repository,
    u.username AS owner
FROM repositories r
JOIN users u ON u.id = r.owner_id;
```


### 2. List all collaborators of a repository.

example : `postgres-learning`
```sql
SELECT
    u.username,
    c.role
FROM collaborations c
JOIN users u ON u.id = c.user_id
JOIN repositories r ON r.id = c.repo_id
WHERE r.name = 'postgres-learning';
```


### 3. Find repositories a user collaborates on.

example : `Neha`

```sql
SELECT r.name
FROM collaborations c
JOIN users u ON u.id = c.user_id
JOIN repositories r ON r.id = c.repo_id
WHERE u.username = 'Neha';
```


### 4. Count collaborators per repository.


```sql
SELECT
    r.name,
    COUNT(c.user_id) AS collaborators
FROM repositories r
LEFT JOIN collaborations c ON r.id = c.repo_id
GROUP BY r.name
ORDER BY collaborators DESC;
```

### 5. Find repository with most collaborators.


```sql
SELECT
    r.name,
    COUNT(c.user_id) AS total
FROM repositories r
JOIN collaborations c ON r.id = c.repo_id
GROUP BY r.name
ORDER BY total DESC
LIMIT 1;
```

### 6. Find users collaborating on more than one repository.

```sql
SELECT
    u.username,
    COUNT(c.repo_id) AS repo_count
FROM users u
JOIN collaborations c ON u.id = c.user_id
GROUP BY u.username
HAVING COUNT(c.repo_id) > 1;
```

### 7. Find users not collaborating on any repository.

```sql
SELECT u.username
FROM users u
LEFT JOIN collaborations c ON u.id = c.user_id
WHERE c.user_id IS NULL;
```

### 8. Find repositories with no collaborators.


```sql
SELECT r.name
FROM repositories r
LEFT JOIN collaborations c ON r.id = c.repo_id
WHERE c.repo_id IS NULL;
```

### 9. List collaborators with repository name and role.

```sql
SELECT
    u.username,
    r.name,
    c.role
FROM collaborations c
JOIN users u ON u.id = c.user_id
JOIN repositories r ON r.id = c.repo_id;
```


### 10. List repositories and number of collaborators.

```sql
SELECT
    r.name,
    COUNT(c.user_id)
FROM repositories r
LEFT JOIN collaborations c ON r.id = c.repo_id
GROUP BY r.name;
```