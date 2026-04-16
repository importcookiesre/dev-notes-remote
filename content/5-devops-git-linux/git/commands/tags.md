

A tag is a named pointer to a specific commit and used like a container.
Analogy : 
	It wraps code till a specific commit and labels it.
	Now external registries can fetch that piece of code.


flow
```go
main branch
│
├── commit A  → login feature
├── commit B  → JWT auth
├── commit C  → bug fixes
│     ↑
│    tag: v1.0.0
│
├── commit D  → caching
├── commit E  → rate limiting
│     ↑
│    tag: v1.1.0
```


## Types of tags : 

### 1. Lightweight tag : 

- Just a name -> commit
- No extra metadata

```go
git tag v1.0.0
```

### 2. Annotated tag : recommanded

- message
- togger name/email
- date
- stored as a full git object

```go
git tag -a v1.0.0 -m "First stable release"

# tag a specific commit
git tag -a v1.0.1 9fceb02 -m "Bugfix release"

```


## commands : 

- List all tags : `git tag`
- Show all details of a tag: `git show v1.0.0`
- Push tag to github : `git push origin v1.0.0`
- Push all tags to github : `git push origin --tags`
- create a branch from tag : `git checkout -b hotfix-v1.0.0 v1.0.0`
	Used when:
	- You need to fix an old production version

