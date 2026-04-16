
##  📂 **File Test Operators**

**File Test Operators** in Bash are used to **check the properties of files and directories**, such as whether a file exists, is readable, writable, executable, or is a directory, etc.

| Operator | Description                                | Example           |
| -------- | ------------------------------------------ | ----------------- |
| `-e`     | File **exists**                            | `[ -e filename ]` |
| `-f`     | File exists and is a **regular file**      | `[ -f filename ]` |
| `-d`     | File exists and is a **directory**         | `[ -d dirname ]`  |
| `-r`     | File is **readable**                       | `[ -r filename ]` |
| `-w`     | File is **writable**                       | `[ -w filename ]` |
| `-x`     | File is **executable**                     | `[ -x filename ]` |
| `-s`     | File exists and is **not empty**           | `[ -s filename ]` |
| `-L`     | File is a **symbolic link**                | `[ -L filename ]` |
| `-b`     | File is a **block device**                 | `[ -b filename ]` |
| `-c`     | File is a **character device**             | `[ -c filename ]` |
| `-p`     | File is a **named pipe (FIFO)**            | `[ -p filename ]` |
| `-S`     | File is a **socket**                       | `[ -S filename ]` |
| `-N`     | File has been **modified** since last read | `[ -N filename ]` |
| `-O`     | File is **owned by the current user**      | `[ -O filename ]` |
|          |                                            |                   |
### 🧪 **Test Use Cases**

|Use Case|Condition|
|---|---|
|Check if a directory exists|`[ -d "$dir" ]`|
|Ensure a config file is present|`[ -f "/etc/myapp.conf" ]`|
|Validate if a script is executable|`[ -x "./deploy.sh" ]`|
|Skip empty files|`[ -s "$logfile" ]`|
|Ensure writable backup location|`[ -w "$backup_dir" ]`|

```bash
#!/bin/bash

file="myfile.txt"

if [ -e "$file" ]; then
    echo "$file exists."

    if [ -f "$file" ]; then
        echo "It's a regular file."
    fi

    if [ -s "$file" ]; then
        echo "The file is not empty."
    else
        echo "The file is empty."
    fi

    if [ -r "$file" ]; then
        echo "You have read permission."
    fi

    if [ -w "$file" ]; then
        echo "You have write permission."
    fi

    if [ -x "$file" ]; then
        echo "You have execute permission."
    fi
else
    echo "$file does not exist."
fi
```

check if file does not exists ( `!` ):
```bash
if [[ ! -f fldr ]]; then 
    echo "file does not exists"
else    
    echo "file exists"
fi
```