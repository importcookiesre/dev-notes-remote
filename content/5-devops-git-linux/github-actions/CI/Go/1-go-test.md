## **What this CI is checking**

This job validates:
- code compiles enough to run tests
- database-dependent tests work
- concurrency issues via race detector
- test execution is fresh, not stale cache
- coverage reports are generated
- coverage gets published

That is a strong backend CI baseline.


```yml
name: CI

on:
  pull_request:
    branches: ["main", "prod", "master"]

  push:
    branches: ["main", "prod", "master"]


env:
  GO_VERSION: "1.24.x"
  NODE_VERSION: "22"

jobs:
   # Go Backend Test Job
  go-tests:
      name: Go Tests
      runs-on: ubuntu-latest
      timeout-minutes: 10

      services:
          postgres:
              image: postgres:18-alpine
              env:
                  POSTGRES_USER: postgres
                  POSTGRES_PASSWORD: mysecretpassword
                  POSTGRES_DB: techshastra
              options: >-
                  --health-cmd "pg_isready -U postgres -d techshastra"
                  --health-interval 10s
                  --health-timeout 5s
                  --health-retries 5
              ports:
                  - 5434:5432

      steps:
          - name: Checkout code
            uses: actions/checkout@v4

          - name: Set up Go
            uses: actions/setup-go@v5
            with:
                go-version: ${{ env.GO_VERSION }}
                cache-dependency-path: apps/server/go.sum

          - name: Cache Go modules
            uses: actions/cache@v4
            with:
                path: |
                    ~/.cache/go-build
                    ~/go/pkg/mod
                key: ${{ runner.os }}-go-${{ hashFiles('apps/server/go.sum') }}
                restore-keys: |
                    ${{ runner.os }}-go-

          - name: Download dependencies
            working-directory: apps/server
            run: go mod download

          - name: Install gotestsum
            run: go install gotest.tools/gotestsum@latest

          - name: Run Go unit tests with gotestsum (race + coverage)
            working-directory: apps/server
            env:
                DATABASE_URL: postgres://postgres:mysecretpassword@postgres:5434/techshastra
                CGO_ENABLED: 1
            run: |
                go clean -testcache

                gotestsum --format=short-verbose \
                  -- -race -count=1 -p=1 -coverprofile=coverage.out -covermode=atomic ./...

                gotestsum --format=short-verbose \
                  -- -race -count=1 -coverprofile=coverage-internal.out -covermode=atomic ./internal/...

          - name: Upload coverage reports
            uses: codecov/codecov-action@v4
            with:
                files: |
                    apps/server/coverage.out
                    apps/server/coverage-internal.out
                flags: unittests
                name: go-coverage
                fail_ci_if_error: false
```

## Job: `go-tests`

This job runs on `ubuntu-latest` and times out after 10 minutes.

### 1) Postgres service

```
services:  
  postgres:  
    image: postgres:18-alpine
```

This starts a Postgres 18 container alongside the job.

It sets:

- user: `postgres`
- password: `mysecretpassword`
- database: `techshastra`

The health check:

```
--health-cmd "pg_isready -U postgres -d techshastra"
```

tells GitHub Actions to wait until Postgres is ready before running later steps.

The port mapping:

```
ports:  
  - 5434:5432
```

means container port `5432` is exposed on runner port `5434`.

### Important detail

Because this job runs directly on `ubuntu-latest` and **not inside a job container**, your app/tests usually connect through `localhost:5434`, not `postgres:5434`.

So this line is likely suspicious:

```
DATABASE_URL: postgres://postgres:mysecretpassword@postgres:5434/techshastra
```

In many GitHub Actions setups, that should be:

```
DATABASE_URL: postgres://postgres:mysecretpassword@localhost:5434/techshastra
```

The `postgres` hostname is typically used when the job itself runs in a container. On a normal hosted runner, `localhost` is the usual choice.

---

## Steps explained

### Checkout code

```
uses: actions/checkout@v4
```

Pulls your repository into the runner.

### Set up Go

```
uses: actions/setup-go@v5  
with:  
  go-version: ${{ env.GO_VERSION }}  
  cache-dependency-path: apps/server/go.sum

Installs Go `1.24.x`.
```

The `cache-dependency-path` tells setup-go which `go.sum` file to use for dependency cache invalidation in a monorepo-style layout.

### Cache Go modules

```
uses: actions/cache@v4  
with:  
  path: |  
    ~/.cache/go-build  
    ~/go/pkg/mod
```

This caches two things:

- `~/go/pkg/mod` → downloaded Go modules
- `~/.cache/go-build` → compiled package/object cache

That speeds up later runs a lot.

### Download dependencies

```
working-directory: apps/server  
run: go mod download
```

This pulls all module dependencies up front so later test steps do not waste time resolving modules.

### Install gotestsum

```
run: go install gotest.tools/gotestsum@latest
```
Installs `gotestsum`, which gives nicer test output than plain `go test`.

One production note: `@latest` is convenient, but not fully reproducible. Pinning a version is safer.

### Run Go tests with race + coverage

```yml
working-directory: apps/server  
env:  
  DATABASE_URL: postgres://postgres:mysecretpassword@postgres:5434/techshastra  
  CGO_ENABLED: 1  
run: |  
  go clean -testcache  
  
  gotestsum --format=short-verbose \  
    -- -race -count=1 -p=1 -coverprofile=coverage.out -covermode=atomic ./tests/...  
  
  gotestsum --format=short-verbose \  
    -- -race -count=1 -coverprofile=coverage-internal.out -covermode=atomic ./internal/...
```

This is the heart of the job.

#### What each flag does

- `go clean -testcache`  
    Clears Go’s test cache so tests rerun fresh.
- `-race`  
    Enables the race detector. Great for catching concurrent access bugs.
- `-count=1`  
    Forces tests to run instead of using cached results.
- `-p=1`  
    Runs package tests one at a time. This is often used when tests hit shared resources like a database.
- `-coverprofile=...`  
    Writes coverage data to a file.
- `-covermode=atomic`  
    Required when using race detection; it gives safe coverage counting under concurrency.

#### Why two `gotestsum` commands?

You are running coverage separately for:

- `./tests/...`
- `./internal/...`

That suggests your repo has different test areas or you want separate coverage files for different packages.

---

## Upload coverage to Codecov

```yml
uses: codecov/codecov-action@v4  
with:  
  files: |  
    apps/server/coverage.out  
    apps/server/coverage-internal.out  
  flags: unittests  
  name: go-coverage  
  fail_ci_if_error: false
```


This sends the coverage reports to Codecov.

`fail_ci_if_error: false` means: if Codecov upload fails, the CI job still passes. That keeps a coverage-upload problem from blocking your whole PR.**