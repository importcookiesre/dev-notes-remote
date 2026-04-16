

- Stores: **TRUE / FALSE**
- Also accepts:
    - `true`, `false`
    - `t`, `f`
    - `1`, `0`
    - `'yes'`, `'no'`


```sql
CREATE TABLE users (
    id BIGSERIAL,
    is_active BOOLEAN
);

INSERT INTO users (is_active)
VALUES (true), (false);
```


```SQL
INSERT INTO users (is_active)
VALUES 
(true),
(false),
('t'),
('f'),
(1),
(0),
('yes'),
('no');
```