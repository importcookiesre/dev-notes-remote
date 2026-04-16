![[Pasted image 20260403075128.png]]

A Bloom filter is a probabilistic data structure that answers a very specific question - have I seen this thing before? - while using almost no memory.

There is a trade-off, though - bloom filters can be ‘wrong’. A Bloom filter will never tell you something is absent when it is actually present, but it might occasionally claim something exists when it does not.

Simply:

- **No false negatives**: if it says “not present,” it is definitely not present.
- **Possible false positives**: The element is likely to exist; however, this is not guaranteed.

**Fundamental Principle :** A Bloom filter is primarily used to reduce system load and enhance performance; however, it should never be treated as a source of truth.

  

---

## Key Characteristics, trade-offs :

- **No False Negatives:** If the filter says an item is not present, it is definitely not there.
- **Possible False Positives:** If it says an item is present, it might be wrong (1 out of N).
- **Space-Efficient:** Uses a small, fixed-size bit array (initially all 0s) to represent large sets.
- **Irreversible:** Items can be added, but not removed.
- **Structure:** Uses k independent hash functions to map an element to k positions in an m-bit array, setting those positions to 1.

### Why bloom filters are used ?

Bloom Filters are ideal for scenarios where:

1. **Memory is a constraint:** They use much less space than other data structures like hash tables.
2. **Speed is crucial:** They offer constant-time O(1) complexity for insertion and lookup.
3. **False positives are acceptable:** As long as the application can tolerate a small percentage of errors, Bloom Filters are a great choice.

  

---

## How it works ?

A Bloom filter has two parts:

1. **Bit array** of size m All bits start as 0.
2. **hash functions** Each item is passed through k number of hash functions, each producing an index in the bit array.

See content credentials

![Article content](https://media.licdn.com/dms/image/v2/D5612AQHa1QY71dQ6Lw/article-inline_image-shrink_1000_1488/B56Z1NisXOKsAQ-/0/1775122434852?e=1776902400&v=beta&t=fYtPPuuDUZPNhtljWuIu5Dfi5yfZR2EXfmFEZ60yEQw)

Bloom Filter Data Processing Diagram

### 1. Insert

When you insert "apple":

- hash1("apple") → 3
- hash2("apple") → 17
- hash3("apple") → 42 Set bits at positions 3, 17, and 42 to 1.

### 2. Query

When you check "apple" again:

- compute the same k positions
- if **any** of those bits is 0, it was **never inserted**
- if **all** are 1, it is **probably inserted** That “probably” matters because other items may have set those bits too.

### 3. Why false positive happens ?

Suppose:

1. "apple" sets bits 3, 17, 42
2. "mango" sets bits 17, 50, 99

Now if you query "banana" and its hashes are 17, 42, 99, all those bits might already be 1 even though "banana" was never inserted.

That is a **false positive**.

  

---

## What are the Trade-offs :

1. **size vs Accuracy :** By increasing the size of the bit array, we can increase the accuracy.
2. **Hash functions:** More hash functions increases accuracy but also increases latency.

**Therefore :**

- More bits → fewer false positives
- More hash functions → better up to a point, then slower and sometimes worse
- More inserted items → more collisions → more false positives

  

---

## Components of Bloom Filter :

A bloom filter uses m size bit array with k number of hash functions to map k bits on each input.

- m = number of bits
- k = number of hash functions
- n = number of inserted items

1. **Bit array**
2. **Hash Functions** A Bloom filter uses k hash functions.
3. **Number of hash functions :** k
4. **Size of the bit array** m

  

---

## Use Cases :

It is a good choice where small number of false positives is tolerable.

1. **Cache or database pre-checks / Database Query Optimization** Before hitting Redis, Postgres, or another expensive store, check whether the item is likely present. If the filter says “definitely not,” you skip the lookup. Used in Apache Cassandra
2. **Malicious URL, span, blacklist lookup :** Quickly test whether an email, IP, domain, or hash is in a known denylist.
3. **Username/email check :** Verifying if a username or email exist in the db or not.
4. **Distributed systems membership hints** A node can quickly ask, “might this data exist on this shard/peer?” before doing a costly network call.
5. **checking whether a URL was already crawled.**

  

---

## The Problem : How bloom filter is rebuilt on cold start ?

Since Bloom filter is a data structure which runs in-memory therefore when the server restarts, it looses all it data. Therefore now on every call to the Bloom filter will return that the data does not exist even though it exists.

**There are various ways to rebuilt it:**

A Bloom filter can be rebuilt on a cold start by scanning the underlying persistent data source like a Database and re-inserting every existing element into a new, empty bit array.

### Then you might have a question : why we can't not persist the data in storage periodically and restore on restart just like Redis does ?

Yes, you **can** persist a Bloom filter and restore it after restart. That is absolutely possible. However, you have to understand that Bloom filter is usually treated as a cache-like optimization and it is never the source of truth.

---

## What to learn next ?

Bloom filter is one of the most widely used probabilistic data structure. It is simple, efficient and uses less memory.

If you’re feeling confident with how Bloom filters work, next you could dive into tuning them for real systems. Learn how to choose the optimal size and number of hash functions. You could also explore variations like counting Bloom filters or scalable Bloom filters, which grow dynamically.

Study these alternatives :

1. Counting Bloom Filters
2. Scalable Bloom Filters
3. Cuckoo Filters