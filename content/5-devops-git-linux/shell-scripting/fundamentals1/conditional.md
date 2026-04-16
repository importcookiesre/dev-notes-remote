

# 🧱 **Types of Conditional Statements in Bash**

1. `if` … `then` … `fi`
2. `if` … `then` … `else`
3. `if` … `elif` … `else`
4. `case` … `esac`


# 1. if
```shell
num=10

if [ $num -gt 5 ]; then
    echo "$num is greater than 5"
fi
```

2. `if` `else`
```shell
read -p "Enter a number: " n

if [ $n -eq 0 ]; then
    echo "Zero"
else
    echo "Non-zero"
fi
```

3. `if elif else`
```shell
score=75

if [ $score -ge 90 ]; then
    echo "Grade: A"
elif [ $score -ge 75 ]; then
    echo "Grade: B"
else
    echo "Grade: C"
fi
```

4. check based on command 

```shell
#!/bin/bash

read -p "Website: " site

# Directly check if ping succeeds
if ping -c 1 "$site"; then
    echo "Successfully connected"
else
    echo "Failure"
fi

```

| Form                     | Subshell? | Keeps variable changes? | Recommended |
| ------------------------ | --------- | ----------------------- | ----------- |
| `if ping -c 1 "$site"`   | ❌ No      | ✅ Yes                   | ✅ Yes       |
| `if (ping -c 1 "$site")` | ✅ Yes     | ❌ No                    | ❌ No        |
# switch case : 

