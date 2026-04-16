
🔹 **1. Declaring and Printing Strings**
```
name="Suraj"
echo "Hello, $name"
```

## 🔹 **2. Concatenation**
Concatenating strings means joining them together.
```
str1="Hello"
str2="World"
greeting="$str1 $str2"
echo $greeting
```

## 🔹 **3. String Length**

Use `${#string}` to get the length.
```
str="Hello"
echo "Length: ${#str}"  # Output: 5
```

## 🔹 **4. Extracting Substrings**

Use `${string:start:length}` format.
```
str="SurajGorai"
echo "${str:0:5}"   # Output: Suraj
echo "${str:5:5}"   # Output: Gorai
```

## 🔹 **5. Substring Search**

Find if a string contains a substring:
```
str="My name is Suraj"
if [[ "$str" == *"Suraj"* ]]; then
  echo "Found!"
fi
```

## 🔹 **6. Replace Substring**

Use pattern replacement:
```
str="I like Linux"
echo "${str/Linux/Unix}"  # Output: I like Unix
```
- `${string/pattern/replacement}` → replace first match
- `${string//pattern/replacement}` → replace all matches

## 🔹 **8. Check if String is Empty or Not**

```
str=""
if [[ -z "$str" ]]; then
  echo "Empty String"
fi

if [[ -n "$str" ]]; then
  echo "Non-empty String"
fi
```
- `-z` → true if string is **zero length**
- `-n` → true if string is **non-zero length**

🔹 **9. String Case Conversion (Bash 4+)**
```
# lowercase
str="HELLO"
echo "${str,,}"    # Output: hello

# uppercase
str="hello"
echo "${str^^}"    # Output: HELLO
```

🔹 **10. Trimming Strings (Leading/Trailing Whitespace)**

```bash
str="   Hello World   "
# Remove leading/trailing spaces using parameter expansion (Bash 4.4+)
trimmed="${str#"${str%%[![:space:]]*}"}" # Trim leading
trimmed="${trimmed%"${trimmed##*[![:space:]]}"}" # Trim trailing
echo ">$trimmed<"
```

🔹 **11. Splitting Strings**
```bash
str="apple,banana,grape"
IFS=',' read -ra fruits <<< "$str"
for fruit in "${fruits[@]}"; do
  echo "$fruit"
done
```
- `IFS` → Internal Field Separator
- `read -ra` → reads into an array