###  Commands:
1. ==`git init`== : Initiate a git repo.
2. ==`git add <fileName>`== : Stages changes (new or modified files) in the working directory for the next commit.
3. ==`git restore --staged <file>`== : Unstage the files. This will remove the file from the staging area, keeping the changes in the working directory but excluding them from the next commit. 
4. ==`git restore <file>`== : It moves to the last staged state. It is used to discard local changes in a file that haven’t been staged yet. This command resets the file to match the latest commit in the current branch, effectively discarding any modifications you made in the working directory. ( It can be reversed with `Ctrl + Z` ).
5. ==`git clean -fd`== : remove all untrack files.
6. ==`git status`== : Shows the current status of the working directory and staging area, including untracked, modified, or staged files.
7. ==`git commit -m "message"`== : Records a snapshot of the staged changes in the repository’s history.
#### Commit History

1.  ==`git log`== : This command displays the commit history for the current branch.
3. ==`git log --oneline`== : This shows a condensed view with each commit on a single line, displaying the commit ID and message.
4. ==`git log -n <number>`== : Replace `<number>` with the number of commits you want to see, e.g., `git log -n 5` to show the last 5 commits.
5. ==`git log <file_name>`== : Show commit for a specific file.
6. ==`git shortlog`== : It gives a summary of commits grouped by authors.
#### Commit Playground :
1. ==`git reset <commit id>`==  :  Moves the current branch to a specified commit, and all the changes are now un-staged, allowing you to stage them into one commit or remove changes from the history. 
   All the changes are now un-staged, so to move to the previous state use  ==`git restore <file>`== .
3. ==`git reset --hard <commit_id>`== : Moves to the specified commit id. It is same as performing both git reset and restore.
4. ==`git rebase i <commit id>`== :  ==Note==
#### Git Stash 
* Stash means store (something) safely in a hidden or secret place.
* `git stash` is a command that temporarily saves changes (tracked files) that you’ve made to your working directory but haven’t committed yet.

1. ==`git stash`== : Temporarily saves changes that haven’t been committed. Allows you to work on something else without committing incomplete work.
2. ==`git stash pop`== : It applies and removes the most recent stash from the list.
3. ==`git stash pop stash@{2}`== : It applies and removes a specified stash.
4. ==`git stash apply stash@{1}`== : It moves to a specified stash.
5. ==`git stash list`== : Lists saved stashes with identifiers.
6. ==`git stash drop stash@{index}`== : Deletes a specified stash.
7. =="git stash clear "== : Clears the stash list.
#### Git Branch
* A **branch** is essentially a pointer to a specific commit in the repository history. Branches allow you to work on different versions or features of a project independently, without affecting the main codebase.
* This allows you to keep code organized and merge it into the main branch only when it's ready.
* When a new branch is created, it contains all the commits from the main branch up to the point of creation along with changes from the working directory. 
1. ==`git branch branch_name`== : Creates a new branch called `branch_name` from the current branch but does not switch to it.
2. ==`git checkout branch_name`== : Switches to `branch_name`. This updates your working directory to reflect the branch's latest commit.
3. ==`git checkout -b branch_name`== : Creates a new branch and immediately switches to it.
4. ==`git branch`== : Displays all branches in your repository. The current branch will have an asterisk (`*`) next to it.
5. ==`git branch -m old_branch_name new_branch_name`== : Renames the branch from `old_branch_name` to `new_branch_name`.
6. ==`git branch -d branch_name`== :  Deletes a branch locally. 
7. ==`git branch -D branch_name`== : Git prevents deleting branches with unmerged changes by default. To force-delete
8. ==`git merge branch_name`== : Merges changes from `branch_name` into the branch you're currently on. Git attempts to merge automatically but may require conflict resolution if there are conflicting changes.
9. ==`git push origin branch_name`== : Pushes `branch_name` to the remote repository, making it available for others to collaborate.
10. ==`git push origin --delete branch_name`== : Deletes a branch from the remote repository.
11. ==`git branch -v`== : Lists all branches with the most recent commit on each branch.
12. ==`git branch --merged`==  : Lists branches that have already been merged into the current branch.
13. ==`git branch --merged`== : Lists branches that have already been merged into the current branch.
14. ==`git branch --no-merged`== : Lists branches that have not been merged into the current branch.
15. ==`git push --set-upstream origin branch_name`== : Sets the remote tracking branch for `branch_name` so that you can use just `git push` or `git pull` without specifying `origin` and `branch_name`.
16. ==`git branch -r`== : Lists all branches on the remote repository.
17. ==`git branch -a`== : Lists all branches in both local and remote repositories.
18. ==`git diff branch1 branch2`== : Shows the differences between two branches.
19. ==`git stash branch branch_name`== : Creates a new branch from the most recent stash and applies the stash to it.
20. ==`git push origin --delete feature-branch`==: Delete a remote branch. 

#### .gitignore

- If files are already pushed before creating gitignore file then it will track those files even if they are excluded in the gitignore
```
# Remove tracked files that are now in .gitignore
git rm -r --cached .

```

#### Git Remote

1. git remote add origin <link> : Links a local repo to a remote repo.
2. Setup upstream : git push -u origin main
3. git remote set-url origin <new-repo-URL>: Update the URL of the git remote
4. git remote remove origin : delete remote url
