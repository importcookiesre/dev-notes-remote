
##  syntax : 

```shell
# Form 1: Recommended
function_name() {
    # commands
}

# Form 2: Less common
function function_name {
    # commands
}
```

**example**
```shell
greet() {
     echo "Hello, Suraj!"
}
greet  # Function call
```


## **Passing arguments to functions**
- Like command-line arguments (`$1`, `$2`, ...), function arguments are passed positionally.
- `$#` returns number of arguments.
```shell
greet() {
    echo "Hello, $1!"
}
greet "Suraj"

add() {
    echo "Sum: $(($1 + $2))"
}
add 5 7
```
### 🧠 Notes:
- `$0`: Script name (file name)
- `$1`, `$2`, ...: Positional parameters
- `$@` or `$*`: All parameters
**pass argument to script**
```shell
echo "first arg : $1"
echo "first arg : $2"
echo "first arg : $3"
```

```
bash myscript.sh arg1 arg2 arg3
```

**pass array to function :** 

```shell
print_array() {
    local arr=("$@")
    for item in "${arr[@]}"; do
        echo "$item"
    done
}

my_arr=("one" "two" "three")
print_array "${my_arr[@]}"
```

## **Returning values from function**

🔸 1. Using `echo` (Common)

```shell
get_name() {
    echo "Suraj"
}
name=$(get_name)
echo "Hello, $name"
```
> `echo` + command substitution is used to **capture output**.

🔸 2. Using `return` (Only for exit status: 0–255)

```bash
is_even() {
    if (( $1 % 2 == 0 )); then
        return 0
    else
        return 1
    fi
}

is_even 10
if [[ $? -eq 0 ]]; then
    echo "Even"
else
    echo "Odd"
fi
```
`return` is **not used to return data**, but for signaling success/failure.


## 🔀 **Local Variables in Functions**

By default, variables are **global** in shell. Use `local` keyword to restrict scope.

```shell
get_name(){
	local age=22
	echo "suraj"
}
echo $age  # cannot access the variable
```

example
```shell
sum_to_n() {
    local total=0
    for ((i=1; i<=$1; i++)); do
        ((total += i))
    done
    echo $total
}

result=$(sum_to_n 100)
echo "Sum from 1 to 100 is $result"

```


##  summary : 

| Feature           | Syntax / Example                       |
| ----------------- | -------------------------------------- |
| Define a function | `myfunc() { ... }`                     |
| Call a function   | `myfunc`                               |
| Arguments         | `$1`, `$2`, `"$@"`, `"$*"`             |
| Return a value    | `echo "val"` and capture with `$(...)` |
| Return status     | `return 0` / `return 1`                |
| Local variables   | `local var=value`                      |
| All arguments     | `$@` or `$*`                           |
| number of args.   | `$#`                                   |

---