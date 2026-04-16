

## 1. delete

1. Delete a remote branch : 
```bash
git push origin --delete fix/config
```

2. Delete branch locally : 
```bash
git branch -d branch_name
git branch -D branch_name # Git prevents deleting branches with unmerged changes by default. To force-delete
```


## 2. create

1. ==`git checkout -b branch_name`== : Creates a new branch and immediately switches to it.
2. `git branch branch_name` : creates a branch but does not switch to it.



## 3. Get branch : 

1. ==`git branch`== : Displays all branches in your repository. The current branch will have an asterisk (`*`) next to it.

2. Lists all branches with the most recent commit on each branch.
```bash
git branch -v
```

3. List branch that are already merged / and not merged into the current branches : 
```bash
git branch --merged  # already merged list
git branch --no-merged  # not merged list
```

4. List all remote branches : 
```bash
git branch -r
```

5. List all remote and local branches : 

```bash
git branch -a
```

6. show difference betweeen two branches : 
```bash
git diff branch1 branch2
```

7. create a new branch from the most recent stash and applies the stash to it.
```bash
git stash branch branch_name
```


## 4. update 

1. rename a branch : 
	- we can rename a branch even if we are not in that branch. (without checking out)

locally :

```bash
git branch -m old_name new_name
```

rename a remote branch :
1. Push the renamed branch and set the upstream: 
2. delete the old branch :
3. verify the changes :

```bash
git push origin -u <new_name>
git push origin --delete <old_name>
git branch -a
```


## 5. merge : 



1. Merges changes from `branch_name` into the branch you're currently on. 
	1. Git attempts to merge automatically but may require conflict resolution if there are conflicting changes.
```bash
git merge branch_name
``` 

2. 
