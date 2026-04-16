
use `act`  to run github actions locally.

first install it locally.



## run : 

### 1. run all workflows : 

```go
act
```

### 2. run a specific event : 

```go
act push
act pull_request
```


### 3. run a specific job : 

```go
act -j go-tests
```


### 4. dry run 

```go
act -n
```


## 2. secret : 


### 1.  CLI

```go
act -s DATABASE_URL=postgres://...
```

### 2. `.secrets` file : recommanded

```go
nano .secrets

DATABASE_URL=postgres://user:pass@localhost:5432/testdb
JWT_SECRET=supersecret
```

run : 
```go
act --secret-file .secrets
```


