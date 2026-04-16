


## 1.  [1757. Recyclable and Low Fat Products](https://leetcode.com/problems/recyclable-and-low-fat-products/)

### solution : 

- postgres converts Products to products. lowercase if they are not in double quote.
```postgresql
SELECT product_id
FROM products
WHERE low_fats = 'Y' and recyclable = 'Y'
```

```postgresql
SELECT product_id
FROM Products
WHERE low_fats = 'Y' and recyclable = 'Y'
```


## 2. [584. Find Customer Referee](https://leetcode.com/problems/find-customer-referee/)



### solution : 

`referee_id = null`  is a wrong syntax.

```python
SELECT 
    name
FROM
    customer
WHERE
    referee_id != 2 OR referee_id IS NULL;
```




## 3. [595. Big Countries](https://leetcode.com/problems/big-countries/)


### solution : 


```sql

```
## 4. [1148. Article Views I](https://leetcode.com/problems/article-views-i/)

### solution : 
```python
select distinct author_id as id
from Views
where viewer_id = author_id
order by author_id;
```


## 5. [1683. Invalid Tweets](https://leetcode.com/problems/invalid-tweets/)

### solution : 

- use `LENGTH(column_name)`  to find the length of the content.
- synonyms of LENGTH() are : CHAT_LENGTH(), CHARACTER_LENGTH().

```sql
SELECT tweet_id
FROM tweets
WHERE LENGTH(content) > 15;
```

## 5. 