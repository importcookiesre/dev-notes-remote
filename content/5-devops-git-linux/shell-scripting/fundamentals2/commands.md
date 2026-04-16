 

# sleep : 

```shell
sleep 5 # 5 seconds
sleep 0.5 # 0.5 seconds

sleep 2s     # 2 seconds
sleep 3m     # 3 minutes
sleep 1h     # 1 hour
sleep 2d     # 2 days
```


# exit : 

The `exit` command is used to **terminate** the execution of a shell script or current shell, optionally providing an **exit status code**.

| Code  | Meaning                              |
| ----- | ------------------------------------ |
| `0`   | Success                              |
| `1`   | General error                        |
| `2`   | Misuse of shell builtins             |
| `126` | Command invoked cannot execute       |
| `127` | Command not found                    |
| `130` | Script terminated by Ctrl+C (SIGINT) |
```
exit
exit 0
exit 1
```
- **check exit code of last command**  : `$?`
	- if it is equal to 0 , then it is success otherwise error. 
```shell
#!/bin/bash

read -p "website " site

ping -c 1  $site
if [[ $? -eq 0 ]]; then 
    echo "successfully connected"
else 
    echo "failure"
fi
```

***recommanded***
```bash
#!/bin/bash

read -p "Website: " site

# Directly check if ping succeeds
if ping -c 1 "$site"; then
    echo "Successfully connected"
else
    echo "Failure"
fi
```



# paths : 

- `basename` : 
```
❯ basename /home/suraj/Desktop/devops/shell-scripting
shell-scripting/file.txt
#  output : file.txt
```
- `dirname` : 
```shell
❯ dirname /home/suraj/Desktop/devops/shell-scripting
shell-scripting/file.txt
#  output : /home/suraj/Desktop/devops/shell-scripting
# shell-scripting
```

- realpath : 
	- assumes that the argument exists in the current dir. and returns the complete path.
```shell
realpath file.txt
# /home/suraj/Desktop/devops/shell-scripting
# shell-scripting/file.txt
```

# RANDOM 
```shell
$RANODM # returns a random number
```

# UID
- root user has uuid = 0.
- 
```shell
$UID  # returns the user id
```

```shell
user=$UID
if [[ $user -eq 0 ]]; then
	echo "root user"
else
	echo "non root user"
fi
```
# write output in a file : 

```shell
ls >file.txt  # overwrites output in the file. (creates if not exit)
date >> file.txt # add output to the file(file.txt)
```

# run script in the background

```
./script.sh &
```
This runs `script.sh` in the background, so you can continue using the terminal while it executes.

**Get the list of background scripts :** `jobs`
```shell
[1]+  Running  bash script.sh & # output
```

**kill background job** : 
- using job pid : `kill 1234`
- using job id : `kill %1`

**redirect output to avoid terminal clutter**
```shell
./script.sh > output.log 2>&1 &
```
Explanation:
- `>` redirects standard output.
- `2>&1` redirects standard error to the same place.
- `&` runs the script in the background.

## `nohup` : ignore hangup signal

If you want the script to **keep running even after you close the terminal**, use `nohup`:

```shell
nohup ./script.sh > output.log 2>&1 &
```
This creates a `nohup.out` file by default if no output redirection is given.

