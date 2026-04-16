
## 🧱 **Types of Loops in Bash**

1. `for` loop
2. `while` loop
3. `until` loop
4. Loop control: `break`, `continue`, `exit`


# `for` loop

method 1 : `for in loop`

```shell
for name in Suraj Amit Neha
do
    echo "Hello $name"
done

for i in 1 2 3 4 5
do
	echo "$i"
done


for i in {1..5}
do
	echo "$i"
done
```

method 2 : 

```shell
for ((i=1; i<=5; i++))
do
    echo "Number: $i"
done
```


method 3 : 

```shell
fruits=("apple" "banana" "mango")

for fruit in "${fruits[@]}"
do
    echo "$fruit"
done
```


# while loop :

Repeats commands **as long as** a condition is true.

```shell
count=5
while [[ $count -gt 0 ]]; do
    echo "Countdown: $count"
    ((count--))
done
```

## read file using while loop : 
- this read the file line by line and prints it.
- file name is mentioned after `done` keyword
```shell
while read name 
do
    echo "$name"
done < myfile.txt
```

## read a csv file : 

```csv
id,name,age
1,suraj,22
2,akash,20
3,sukumar,40
4,suravi,36
```

```shell
while IFS="," read f1 f2 f3 
do
    echo "$f1 $f2 $f3"
done < csvfile.csv
```

#  until loop
- Repeats commands **until** a condition becomes true (i.e., while it's false).
- This is the **opposite of `while`**, and runs as long as the condition is **false**.
```shell
until [ -f "myfile.txt" ]
do
    echo "Waiting for file..."
    sleep 1
done

echo "File is now available!"
```


# 🚧 **Loop Control Statements**

| Keyword    | Function                                        |
| ---------- | ----------------------------------------------- |
| `break`    | Exit the loop immediately                       |
| `continue` | Skip the current iteration and jump to the next |
| `exit`     | Terminate the entire script                     |
```shell
for ((i=1; i<=5; i++)); do
    if [ $i -eq 3 ]; then
        continue  # Skip number 3
    fi
    echo "i = $i"
    if [ $i -eq 4 ]; then
        break  # Stop loop at 4
    fi
done

```



# switch case : 

