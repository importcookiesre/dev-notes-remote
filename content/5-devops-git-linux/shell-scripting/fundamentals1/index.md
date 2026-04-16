


# comments

```bash
#!/bin/bash

echo "learning comments"

# this is a single line comment
<<hello
this 
is 
a
multiline
comment
hello
```


# variables : 

- unset variable using `unset var_name`

```bash
name="suraj"
num=10
echo "my name is $name"
```
- No `space` around the `=`
- Access using `$` (e.g., `$name`)

## 2. **Environment / global Variables**

> Predefined system-wide variables that affect process behavior.

```bash
echo $HOME     # Shows your home directory
echo $USER     # Your username
echo $PATH     # System executable search path
```
- Accessible to all child processes
- Use `export` to promote user-defined variable to environment level:
```bash
export name="Suraj # global scope
```

**example**
```bash
#!/bin/bash

export planet="Earth"
./child.sh
```

```shell
#!/bin/bash

echo "Hello from $planet!"
```


## constant

```shell
readonly name="suraj"
```


## command : 
- Store command in a variable and execute using `eval`.
```bash
cmd="ls -l"
eval "$cmd" 
```