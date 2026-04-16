
Instead of writing raw Bash scripts for every single task (which is error-prone and hard to maintain), you "import" a pre-written, well-tested package to do the heavy lifting for you.


```go
- name: Step Name
  uses: author/repo@version   # <--- The Import
  with:                       # <--- Function Arguments
    key: value
```

| Action                              | Purpose       | Why use it                                                                                                  |
| ----------------------------------- | ------------- | ----------------------------------------------------------------------------------------------------------- |
| **`actions/checkout`**              | Git Clone     | Fetches your code. Handles auth tokens automatically.                                                       |
| **`actions/setup-go`**              | Runtime Setup | Installs Go _and_ handles module caching automatically.                                                     |
| **`golangci/golangci-lint-action`** | Linting       | Runs the standard Go linter. Much faster than running `go run ...` yourself because it caches the analysis. |
| **`docker/login-action`**           | Auth          | Securely logs into Docker Hub / AWS ECR.                                                                    |
| **`docker/build-push-action`**      | Build         | Builds Docker images using BuildKit (faster/caching).                                                       |
| **`goreleaser/goreleaser-action`**  | Release       | Automates binary builds, changelogs, and releases for Go projects.                                          |


