




### 1. ignore all folders with a specific name : 

#### solution

`.gitignore`
```bash
**/node_modules/  # ignore all node_modules
**/temp  # ignore all folder with name temp
```

### 2. undo all changes and move to the last commit state.

```bash
git restore .  # undo all changes
git clean -fd  # undo untracked files
```


## 3. commits: 


#### 1. You have git pushed before creating gitignore and it is tracking the files which are not supposed to be : 


```bash
git rm -r --cached .     # untrack all the files
git add .                # add
git commit -m "Clean up tracked files according to .gitignore"
git push
```


### 2. avoid a file in the commit : 

- there are few files that we dont want to commit, so avoid some files in the commit.

- solution
	- First track all the files then untrack those files that are not needed to commit.
	- Hit the commit command.
```bash
git add .
git restore --staged my_notes.md 
git commit -m "commit message"
```


### 3. rollback the last commit and keep the changes : 

- This moves `HEAD` back by one commit but keeps all the changes **staged**.
```bash
git reset --soft HEAD~1
```

- This moves `HEAD` back by one commit but keeps all the changes **unstaged**
```bash
git reset --mixed HEAD~1
```


## 5. clone and pull :

### 1. clone any specific branch 

- There are multiple branches in github but you have to pull any specific branch.
- git bydefault pulls the main branch.

- clone v2 branch
```bash
git clone -b v2 https://github.com/surajgoraicse/node-express-template.git
```
