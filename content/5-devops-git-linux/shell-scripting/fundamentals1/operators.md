
Shell doesn't support operators like C or Python directly, but you can use them through:

- `expr`
- `$(( ))` (Arithmetic Expansion)
- `[[ ... ]]` or `[ ... ]` (Test Command)
- `let`, `test`, and `[[ ]]` for comparisons and logic


## 1. 🧮 **Arithmetic Operators**

Used to perform integer math. Syntax varies:
> In Bash, **arithmetic operators** are used to perform **integer-based mathematical computations**. Arithmetic expressions can be evaluated using constructs like `let`, `$(( ))`, `expr`, and `(( ))`.

> **Note**: Bash only supports **integer arithmetic** natively. For floating-point arithmetic, you need tools like `bc` or `awk`.


### ✅ Syntax Options:
- `expr 1 + 2` (older, less preferred)
- `let "a = b + c"`
- `$(( a+b ))` ✅ Recommended
- `(( a+b ))` (same but used in control structures)

| Operator | Description         | Example                  |
| -------- | ------------------- | ------------------------ |
| `+`      | Addition            | `echo $((5 + 3))` → `8`  |
| `-`      | Subtraction         | `echo $((10 - 4))`       |
| `*`      | Multiplication      | `echo $((2 * 3))`        |
| `/`      | Division (int only) | `echo $((10 / 2))`       |
| `%`      | Modulus             | `echo $((7 % 3))`        |
| `**`     | Power (Bash 4+)     | `echo $((2 ** 3))` → `8` |
```shell
((c=a+b))
product=$((a*b))

echo "$c"
echo "$product"
```
---

## 2. 📊 **Relational Operators**

### Integer : 
Used with `[ ... ]`, `[[ ... ]]`, or `test`.

| Operator | Description      | Example             |
| -------- | ---------------- | ------------------- |
| `-eq`    | Equal to         | `[ "$a" -eq "$b" ]` |
| `-ne`    | Not equal to     | `[ "$a" -ne "$b" ]` |
| `-gt`    | Greater than     | `[ "$a" -gt "$b" ]` |
| `-lt`    | Less than        | `[ "$a" -lt "$b" ]` |
| `-ge`    | Greater or equal | `[ "$a" -ge "$b" ]` |
| `-le`    | Less or equal    | `[ "$a" -le "$b" ]` |

⚠️ These work only for **integers**.

### **String Operators**
Used to test equality, check emptiness, or do lexicographical comparison.

| Operator    | Description               | Example               |
| ----------- | ------------------------- | --------------------- |
| `=` or `==` | String equality           | `[ "$a" == "$b" ]`    |
| `!=`        | String inequality         | `[ "$a" != "$b" ]`    |
| `-z`        | Is string empty           | `[ -z "$var" ]`       |
| `-n`        | Is string non-empty       | `[ -n "$var" ]`       |
| `<`         | Lexicographically less    | `[[ "$a" < "$b" ]]` ✅ |
| `>`         | Lexicographically greater | `[[ "$a" > "$b" ]]` ✅ |

> 🔁 Prefer `[[ ... ]]` for advanced string tests like `<` and `>` to avoid parsing issues.

```bash
str1="suraj"
str2="amit"

if [[ "$str1" > "$str2" ]]; then
    echo "$str1 comes after $str2"
else
	echo "$str1 comes before $str2"
fi
```

---

## 3. 📂 **File Test Operators**

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

| Use Case                           | Condition                  |
| ---------------------------------- | -------------------------- |
| Check if a directory exists        | `[ -d "$dir" ]`            |
| Ensure a config file is present    | `[ -f "/etc/myapp.conf" ]` |
| Validate if a script is executable | `[ -x "./deploy.sh" ]`     |
| Skip empty files                   | `[ -s "$logfile" ]`        |
| Ensure writable backup location    | `[ -w "$backup_dir" ]`     |

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
## 4. 🔁 **Logical Operators**

Used to combine multiple conditions in `if`, `while`, etc.

| Operator | Meaning     | Example                       |
| -------- | ----------- | ----------------------------- |
| `&&`     | Logical AND | `[[ $a -gt 5 && $b -lt 10 ]]` |
| \|\|     | Logical OR  |                               |
| `!`      | Logical NOT | `[[ ! -f file.txt ]]`         |

> These work with `[[ ... ]]` and `(( ... ))` only.

---

## 5. 📝 **Assignment Operators**

Used to assign or update variable values.

|Operator|Meaning|Example|
|---|---|---|
|`=`|Assign|`x=5`|
|`+=`|Add and assign|`((x += 3))`|
|`-=`|Subtract and assign|`((x -= 2))`|
|`*=`|Multiply and assign|`((x *= 4))`|
|`/=`|Divide and assign|`((x /= 2))`|

