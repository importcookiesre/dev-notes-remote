
Steps runs sequentially : 1-2-3


- **All steps in a job run on the same disk.**
- **All steps share the same working directory.**
- Every line of step runs on different shell, so if we set an env in line 2, we cant access it in line 3.
	- Github provides a special keyword for this.


## Types of steps : 


### 1.  `run`

- this executes the command in the runner's os shell.

```go
- name: Install dependencies
  run: go mod download
```

**Multi line** `|` : Essential for complex logic without creating a separate `.sh` file.

### 2. `uses` step : 

This executes a pre-packaged Action (a reusable unit of code).

```go
- name: Checkout Code
  uses: actions/checkout@v4
```

### 3. Communicating between steps : 

Since steps run in different shell processes, setting `export VAR=foo` in Step 1 **will not** work for Step 2. The variable dies when Step 1's shell closes.

You must use **GitHub's Special Files** to persist state.


#### A. Environment variable : `$GITHUB_ENV`

To set env var for all further steps : 

```go
- name: Set dynamic env var
  run: echo "APP_VERSION=v1.2.3" >> "$GITHUB_ENV"

- name: Use it later
  run: echo "Deploying version $APP_VERSION" # Prints v1.2.3
```

#### B. Output : `$GITHUB_OUTPUT`

To pass a specific value to another step (or to the Job outputs):

```go
- name: Generate ID
  id: gen_id                # <--- ID is required to reference it later
  run: echo "request_id=12345" >> "$GITHUB_OUTPUT"

- name: Use ID
  run: echo "Processing ID ${{ steps.gen_id.outputs.request_id }}"
```

### 4. Custom shells : 

We aren't limited to Bash, we can use python or other interpreters in our yml.

**Backend Use Case:** Writing a complex migration script in Python instead of messy Bash.

```go
- name: Run Python Script
  shell: python
  run: |
    import os
    import json
    
    data = {"status": "success", "db": os.getenv("DB_HOST")}
    print(json.dumps(data))
```

### 5. Conditional execution : 

Sometimes you want steps to run _even if the previous step failed_ (e.g., uploading error logs).

**Common Conditions:**
- `if: always()`: Runs no matter what (good for cleanup).
- `if: failure()`: Runs only if a previous step failed.
- `if: success()`: Default behavior.

```go
steps:
  - name: Run Tests
    run: go test ./...
    
  - name: Upload Logs
    if: failure()           # <--- Only runs if 'Run Tests' crashed
    uses: actions/upload-artifact@v4
    with:
      name: error-logs
      path: test-logs.txt
```


### set working dir : 

By default, steps run in the root of your repo. If your Go code is in a `backend/` subdirectory, you don't want to `cd backend` in every step.


```go
steps:
  - name: Test Backend
    working-directory: ./backend  # <--- Changes context to this folder
    run: go test ./...
```