

Stash means store (something) safely in a hidden or secret place.


1. Create a stash from the uncommited changes in the current directory : 
```bash
git stash
```

2. Apply and remote the latest stash:
```bash
git stash pop
```

3. Apply and remove a specific stash:
```bash 
git stash pop stash@{1}
```

4. Apply a specific stash
```bash
git stash pop stash@{1}
```

5. list all stash :
```bash
git stash list
```

6. Delete a specific stash 
```bash
git stash drop stash@{index}
```

7. Delete all stash
```bash
git stash clear
```
