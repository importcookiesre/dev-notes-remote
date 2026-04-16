
## Trace mode :
```
set -x

bash -x myscript.sh # instead of modifying the script run.
```


```shell
#!/bin/bash
set -x  # Enable debug mode

a=5
b=10
c=$((a + b))
echo "Sum is $c"

set +x  # Disable debug mode
```


## Use `set -e` to Catch Failures
- Tells the shell to **exit immediately** if any command fails (non-zero status)
- Helps catch errors early
```
set -e
```

- all these commands will run.
- in case of error, it will print the error and continue with the next commands
```shell
pwd
sdfsjdfjsdf # error command
date
ls
```

using `set -x` we can terminate the script as soon as we encounter an error
```
pwd
sdfsjdfjsdf # script will stop here
date
ls
```