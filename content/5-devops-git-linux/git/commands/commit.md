

## 1. commit :


### 1. restore :

1. Discard changes in the specified directory : 
```bash
git restore <file_name> 
git restore .  # remove changes in all files
```

2. Unstage files : 
```bash
git restore --staged <file>   # unstages a file
```



### 2. reset : 

1. Move the current branch to a specific commit. 
	- all the changes are now un-staged, allowing you to stage them into one commit or remove changes from the history. 
```bash
git reset <commit_id>
```

3. Moves to the specified commit id. It is same as performing both git reset and restore.
```bash
git reset --hard <commit_id>
```





## 2. Commit history :

1. This command displays the commit history for the current branch.
```bash
git log
```

2.  This shows a condensed view with each commit on a single line, displaying the commit ID and message.
```bash
git log --oneline
```

3. Display `n` number of logs : 
```bash
git log -n <number>  # git log -n 5
```

4. Show commits of a specific file :
```bash
git log <file_name>  # git log cmd/main.go
```

5. It gives a summary of commits grouped by authors.
```bash
git shortlog
```



## 3. rebase


## 4. commit message : 


```go
feat(auth): add JWT refresh token support
fix(api): handle nil context in middleware
refactor(db): extract repository interface for users
perf(cache): reduce memory allocation in LRU implementation
test(order): add integration test for failed payment flow
docs(readme): update setup instructions for Docker
```



# Playground 

## 1. how to remove the last commit both locally and remote. You have a PR open on this.


### Option 1 : Hard reset + force push (most common)

1. Remove last commit locally : 

```go
git reset --hard HEAD~1
```

2. Force push : 
```go
git reset --hard HEAD~1
```