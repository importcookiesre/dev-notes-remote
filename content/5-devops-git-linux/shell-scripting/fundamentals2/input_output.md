
# output : 

1. Using `echo`
```bash
echo "Hello, Suraj"
```
✅ Features:
- Automatically adds a newline.
- Can use variables:
```bash
name="Suraj"
echo "My name is $name"
```


### 2. Using `printf`

More powerful and precise than `echo`.
```shell
printf "Name: %s\nAge: %d\n" "Suraj" 21
```
✅ Features:
- Works like C's `printf`
- Requires manual newline (`\n`)
- Good for formatting numbers, tables, etc.

### 3. Ouptut in a file : 

```
echo "output will be over-written in a file " > myfile.txt
ls -l >> myfile.txt
echo "ouput will be discarded" > /dev/null
```
####  What is `/dev/null`?
`/dev/null` is a special **black hole** file in Unix/Linux:
- Any data written to it is **immediately discarded**
- Useful to **suppress output** (both stdout and stderr)

|Purpose|Syntax|
|---|---|
|Discard standard output|`command > /dev/null`|
|Discard error output|`command 2> /dev/null`|
|Discard both outputs|`command > /dev/null 2>&1`|
```
```
# input : 

## 1. Using `read`
Reads input from user (standard input – keyboard).

```shell
read name
echo "You entered: $name"
```

You can also prompt the user:
```shell
#!/bin/bash
read -p "Enter your name: " name
read -p "Enter your age: " age
echo "Hi $name, you are $age years old."
```

### 🔸 `read` Options

| Option | Description                                  |
| ------ | -------------------------------------------- |
| `-p`   | Prompt string                                |
| `-s`   | Silent mode (no echo) — useful for passwords |
| `-t`   | Timeout                                      |
| `-n`   | Number of characters to read                 |
| `-a`   | Read into an array                           |
| `-e`   | Enables readline (arrow keys, etc.)          |


1. Silent password input:
```shell
read -sp "Enter password: " password
echo
```

2. timeout
```shell
read -t 5 -p "Enter your name in 5 seconds: " name
```

3. read into an array
```shell
read -a colors
echo "First color: ${colors[0]}"
```