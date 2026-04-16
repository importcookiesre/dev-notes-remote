
# number

**Note**: Bash only supports **integer arithmetic** natively. For floating-point arithmetic, you need tools like `bc` or `awk`.

# array

Bash supports **one-dimensional indexed arrays** and (in newer versions) **associative arrays**.

```bash
# Declare an array
fruits=("apple" "banana" "cherry")

# or explicitly with 'declare'
declare -a fruits=("apple" "banana" "cherry")
```

## indexed array
```bash
fruits=("apple" "banana" "mango")
echo ${fruits[1]}     # banana
echo ${fruits[@]}     # all elements

#  all values
echo ${fruits[@]}    # apple banana cherry
echo ${fruits[*]}    # apple banana cherry

echo ${#fruits[@]}   # array length

fruits[3]="orange"       # Add new item
fruits[1]="blueberry"    # Modify existing item

# remove 
unset fruits[2]   # Removes "cherry"

# range of values
echo "${fruits[*]:1:3}"
```

| In arrays     | Meaning                         |
| ------------- | ------------------------------- |
| `"${arr[*]}"` | All elements as one string      |
| `"${arr[@]}"` | Each element as separate string |
## associative arrays

- key value pairs

```bash
declare -A capital

capital["India"]="New Delhi"
capital["USA"]="Washington"
capital["France"]="Paris"

echo ${capital["India"]}    # Output: New Delhi
echo "${#capital[@]}" # length
```

```shell
declare -A number 
number=([one]=1 [two]=2 [three]=3)

echo "${number[one]}"
echo "${number[two]}"
echo "${number[three]}"
```



# boolean 

```shell
is_valid=true
is_active=false
```

