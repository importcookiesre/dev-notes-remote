
Check if a port is running :
- ` Run lsof -i :3000 ` : to see if port 3000 is in use. 
	![[Pasted image 20250401063649.png]]
- kill the port using :  `sudo kill -9 PID`
	- sudo kill -9 95828

#### File Handling

| **Command**         | **Purpose**                             | **Example**                      |
| ------------------- | --------------------------------------- | -------------------------------- |
| `ls`                | List directory contents                 | `ls -l` (detailed view)          |
| `cd`                | Change the current directory            | `cd /home/user/Documents`        |
| `pwd`               | Print the current working directory     | `pwd`                            |
| `cp`                | Copy files or directories               | `cp file.txt /home/user/backup/` |
| `mv`                | Move or rename files or directories     | `mv file.txt /home/user/`        |
| `rm`                | Remove files or directories             | `rm file.txt`                    |
| `rm -r folder_name` | -r (recursive) removes a directory.     | `rm -f data`                     |
| `touch`             | Create an empty file                    | `touch newfile.txt`              |
| `mkdir`             | Create a new directory                  | `mkdir new_directory`            |
| `chmod`             | Change file permissions                 | `chmod 755 script.sh`            |
| `sudo`              | Run a command with superuser privileges | `sudo apt update`                |


|**Command**|**Purpose**|**Example**|
|---|---|---|
|`ls`|List directory contents|`ls -l` (detailed view)|
|`cd`|Change the current directory|`cd /home/user/Documents`|
|`pwd`|Print the current working directory|`pwd`|
|`cp`|Copy files or directories|`cp file.txt /home/user/backup/`|
|`mv`|Move or rename files or directories|`mv file.txt /home/user/`|
|`rm`|Remove files or directories|`rm file.txt`|
|`touch`|Create an empty file|`touch newfile.txt`|
|`mkdir`|Create a new directory|`mkdir new_directory`|
|`chmod`|Change file permissions|`chmod 755 script.sh`|
|`sudo`|Run a command with superuser privileges|`sudo apt update`|
