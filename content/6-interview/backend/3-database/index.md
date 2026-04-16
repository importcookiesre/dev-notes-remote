

1. **How do you protect a server from SQL injection attacks?**
	1. Parameterized queries : 
		1. write quries leveing the placeholder which are replaced with input.
	2. Using ORM : 
		1. puts abstraction layer
		2. prevent direct db interaction

2. **Describe how you would implement a full-text search in a database.**
	1. Native Database approch : 
		- **Tokenization & Stemming:** When text is inserted, the database breaks it down into "tokens" (words) and reduces them to their root form (lexemes). For example, "running," "runs," and "ran" all become the lexeme `run`. In Postgres, this is stored as a `tsvector`.
		    
		- **The Query:** The user's search query is similarly parsed into a `tsquery`.
		    
		- **The GIN Index:** The database uses a Generalized Inverted Index (GIN). Instead of indexing a row and its contents, it indexes the _lexemes_ and points back to the row IDs.

	2. Use specilized db like elastic-search, opensearch
	3. Vector Database

3. **What strategies would you use to manage database connections in a high-load scenario?**
	1. Connection Pool : reduce the time required to establish new connection.
	2. Dedicated connection pooler like PgBouncer.
	3. Load sheding and context cancellation
	4. caching 

4. **What is N+1 query problem ?**
	 ==An application executes one initial query to retrieve a list of parent records, and then executes an additional, separate query for each of those N records to fetch associated child data==.
	1. First, fetch all students: `SELECT * FROM students;`
	2. Then, for each student, fetch their courses one by one: `SELECT * FROM courses WHERE student_id = ?;`


