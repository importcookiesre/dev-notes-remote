


## Git Commands Cheatsheet (Organized with Descriptions)

### 🚀 Git Initialization & Basics

| Command                       | Description                                                   |
| ----------------------------- | ------------------------------------------------------------- |
| `git init`                    | Initializes a new Git repository.                             |
| `git status`                  | Displays the state of the working directory and staging area. |
| `git add <file>`              | Stages changes for commit.                                    |
| `git restore --staged <file>` | Unstages a file (keeps changes in working directory).         |
| `git restore <file>`          | Discards changes in the working directory.                    |
| `git clean -fd`               | Removes all untracked files and directories.                  |
| `git commit -m "msg"`         | Commits staged changes with a message.                        |

---

### 📜 Git Commit History

|   |   |
|---|---|
|Command|Description|
|`git log`|Full commit history.|
|`git log --oneline`|Compact view (one line per commit).|
|`git log -n <number>`|Shows the last n commits.|
|`git log <file>`|Commit history for a specific file.|
|`git shortlog`|Summary grouped by author.|

---

### ⏮️ Git Commit Playground

|   |   |
|---|---|
|Command|Description|
|`git reset <commit-id>`|Resets to a commit (unstages changes).|
|`git reset --hard <commit-id>`|Completely resets to a previous commit.|
|`git rebase -i <commit-id>`|Interactive rebase for rewriting history.|

---

### 🧳 Git Stash

|   |   |
|---|---|
|Command|Description|
|`git stash`|Temporarily saves uncommitted changes.|
|`git stash pop`|Applies and removes the most recent stash.|
|`git stash pop stash@{2}`|Applies and removes a specific stash.|
|`git stash apply stash@{1}`|Applies a specific stash (does not remove).|
|`git stash list`|Shows all stashes.|
|`git stash drop stash@{index}`|Deletes a specific stash.|
|`git stash clear`|Clears all stashes.|
|`git stash branch branch_name`|Creates a branch from stash and applies it.|

---

### 🌿 Git Branching

|   |   |
|---|---|
|Command|Description|
|`git branch branch_name`|Creates a new branch.|
|`git checkout branch_name`|Switches to a branch.|
|`git checkout -b branch_name`|Creates and switches to a branch.|
|`git branch`|Lists all local branches.|
|`git branch -v`|Lists branches with latest commit.|
|`git branch -m old new`|Renames a branch.|
|`git branch -d branch_name`|Deletes a branch (safe).|
|`git branch -D branch_name`|Force-deletes a branch.|
|`git merge branch_name`|Merges changes from another branch.|
|`git diff branch1 branch2`|Shows differences between two branches.|
|`git push origin branch_name`|Pushes branch to remote.|
|`git push origin --delete branch_name`|Deletes branch on remote.|
|`git push origin --delete feature-branch`|Deletes a feature branch on remote.|
|`git branch --merged`|Shows branches already merged.|
|`git branch --no-merged`|Shows branches not merged.|
|`git branch -r`|Lists remote branches.|
|`git branch -a`|Lists all branches (local + remote).|
|`git push --set-upstream origin branch_name`|Sets upstream for branch.|

---

### 🌐 Git Remote

|   |   |
|---|---|
|Command|Description|
|`git remote add origin <url>`|Adds a remote repository.|
|`git push -u origin main`|Push with upstream set.|
|`git remote set-url origin <url>`|Changes remote repository URL.|
|`git remote remove origin`|Removes the origin remote.|

---

### ⚙️ .gitignore & Cleanup

|   |   |
|---|---|
|Command|Description|
|`git rm -r --cached .`|Untrack files listed in .gitignore.|

> 🧠 **Note:** `.gitignore` does not remove files already tracked. Use `git rm -r --cached .` to stop tracking them after updating `.gitignore`.

---