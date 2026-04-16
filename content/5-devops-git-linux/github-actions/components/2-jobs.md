A **job** is the **smallest independently schedulable unit** in GitHub Actions.

- Runs independently on one runner.
-  By default Runs parallely and do not share data.



```python
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - run: go test ./...

  lint:
    runs-on: ubuntu-latest
    steps:
      - run: golangci-lint run

  build:
    runs-on: ubuntu-latest
    steps:
      - run: go build ./...
```
- 3 runners spin up
- All jobs run simultaneously
- CI finishes faster


## pipeline : 



- For sequential job running, we use `needs` for creating pipeline : 
To build a pipeline, you need order. You can't "Deploy" before you "Build". The `needs` keyword handles this dependency.


```python
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - run: go test ./...

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - run: go build ./...

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: ./deploy.sh
```


execution order : `test → build → deploy`
- **Fan-in:** We can wait for multiple jobs: `needs: [lint, security, build]`



## service container : 

How do you run integration tests that require a real database? You _could_ install Postgres manually in a step, but that's messy. Instead, use `services`. GitHub spins up a Postgres container, networks it to your job, and destroys it when the job ends.

```go
jobs:
  integration-test:
    runs-on: ubuntu-latest
    
    # SPIN UP POSTGRES SIDE-BY-SIDE
    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_USER: user
          POSTGRES_PASSWORD: password
          POSTGRES_DB: test_db
        ports:
          - 5432:5432
        # Healthcheck matches "pg_isready" to ensure DB is up before steps start
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5

    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-go@v5
      
      # Now your Go code can connect to localhost:5432
      - name: Run Integration Tests
        run: go test -tags=integration ./...
        env:
          DATABASE_URL: postgres://user:password@localhost:5432/test_db?sslmode=disable
```



## matrix strategy : multi dimensional testing 

We want to ensure that our program works on both Go 1.22 and 1.24 and may be on both ubuntu and windows. The `matrix` strategy automatically creates multiple job copies.


```go
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        go-version: ['1.21', '1.22']
        database: ['postgres:14', 'postgres:15']
        
    services:
      db:
        image: ${{ matrix.database }} # Dynamic Image!
        
    steps:
      - uses: actions/setup-go@v5
        with:
          go-version: ${{ matrix.go-version }} # Dynamic Version!
      - run: go test ./...
```

- **Result:** This single block creates **4 Jobs** (2 Go versions × 2 DB versions) running in parallel.


## Passing data  : job outputs 

Since jobs are isolated, Job B cannot read variables from Job A. You must explicitly "throw" data over the wall using **Outputs**.

**Scenario:** Job A builds a Docker image and tags it with a unique ID. Job B needs that ID to deploy it.
### Job A (The Producer):
```go
jobs:
  build:
    runs-on: ubuntu-latest
    outputs:
      # Map the step output to the job output
      docker_tag: ${{ steps.meta.outputs.tag }} 
    steps:
      - id: meta
        run: |
          NEW_TAG="v1.0.${{ github.run_number }}"
          echo "tag=$NEW_TAG" >> "$GITHUB_OUTPUT"
```


#### Job B (The Consumer):
```go
deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Deploy Image
        run: echo "Deploying image with tag: ${{ needs.build.outputs.docker_tag }}"
```


## Environment : the gatekeeper

For a backend engineer, **Deploying to Production** is scary. You don't want it to happen automatically.

You can tag a job with an `environment`. In your GitHub Repository Settings, you can configure "Protection Rules" for that environment (e.g., "Require approval from Senior Engineer").


```go
jobs:
  deploy-prod:
    runs-on: ubuntu-latest
    needs: integration-test
    environment: 
      name: Production
      url: https://api.my-app.com
    steps:
      - run: ./deploy.sh
```

**The Effect:** When the workflow reaches this job, it **pauses**. It sends an email to the required reviewers. The job (and the billing timer) hangs until someone clicks "Approve" in the GitHub UI.