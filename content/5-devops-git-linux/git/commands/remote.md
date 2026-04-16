

## 1. origin : 



# Playground 


## 1. Sync remote branches : 

- We have some branches in github that we dont have locally. Get those branches locally : 

##### solution : 

```bash

git fetch origin 

git branch -a # list all branch
# or git branch -avv (include last commit also)

git branch -r # list all remote branches


# checkout but not create a branch
git checkout origin/<remote_branch_name>

# create a branch
git checkout -b v1  origin/<remote_branch_name>

# verify 
git branch -avv
```

1. `git fetch origin`
	
	- The `git fetch` command downloads all branches and their commit history from the remote repository to your local machine, without automatically merging them with your work.

2. `git branch -a` or `git branch -avv`
	- This will help you see the newly fetched branches. The output will show remote branches prefixed with `remotes/origin/`.

3. `git checkout <branch-name>`

	- To create a local copy and start tracking a remote branch, you can use the `git checkout` or `git switch` command. Git will automatically create a local branch with the same name that tracks the remote version.
	- **For each branch you want to bring locally, run one of the following commands:**

Using `git switch` (recommended for newer versions of Git):
```sh
git switch <branch-name>
```

4.  `git checkout -b v1 origin/v1` 
	- This creates a local branch called `v1`and sets it to track `origin/v1`.
	- You’re now on the `v1` branch locally.


## 2. Remove a remote branch : 

- Delete a branch from github.
- You cannot remove a remote branch if it is set default, you can change it from the settings.


```bash
git push origin --delete remote_branch_name
```


## 3. 
