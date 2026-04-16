

## 1. length of content : LENGTH()

- use octet_length() to calculate bytes : useful when calculating non utf-8 character like emojis etc.
- 
### 1. single column : 

You have a Product table of product names. You need to find the name of the product that has the longest name (in characters).
**Task:** Write a PostgreSQL query to return the `product_name` and its length for the single product with the maximum name length.

| product_id | product_name        |
| ---------- | ------------------- |
| 1          | Laptop Pro          |
| 2          | Mini Mouse          |
| 3          | Keyboard Mechanical |
| 4          | USB-C Adapter       |

#### solution : 

```sql
SELECT
	 product_name, 
	 LENGTH(product_name) AS name_length
FROM
	products
ORDER BY
	name_length DESC
LIMIT 1;
```

### 2. Handling NULL Values

**Scenario:** You are inserting data into a customer table. One of the entries has a `NULL` value for the `email` column.

**Task:** What is the result of applying `LENGTH()` to a `NULL` value in PostgreSQL?

```sql
SELECT LENGTH(NULL) AS length_of_null;
```


### sol : 

- ans : NULL
- The length of a non-existent value (`NULL`) cannot be calculated, so the result is `NULL`.


### 3. Zero-Length Strings vs. NULL

What will be the length of a field which is empty and not null : 

#### soln : 

ans : 0
A zero-length string is an actual string (it exists) that contains zero characters.


### 4. Counting Bytes vs. Characters (Multibyte Characters)

The PostgreSQL database is using `UTF-8` encoding, which supports multibyte characters (like emojis or non-Latin script characters).

**Task:** Which function in PostgreSQL should you use to accurately count the number of _characters_, especially when dealing with multibyte character sets, and how does it relate to `LENGTH()`?


#### **Solution:**

The functions `LENGTH()`, `CHAR_LENGTH()`, and `CHARACTER_LENGTH()` are all synonyms and return the **number of characters** (which is what you typically want).

However, it is important to know about the function `OCTET_LENGTH()`, which counts the **number of bytes**.

```sql
-- Assuming a character like 'é' (e-acute) is represented by 2 bytes in UTF-8
-- or '😊' (smiley face) is represented by 4 bytes.
SELECT
    LENGTH('résumé') AS char_count,
    OCTET_LENGTH('résumé') AS byte_count;
```

| **char_count** | **byte_count** |
| -------------- | -------------- |
| 6              | 7              |


## 2. sdf