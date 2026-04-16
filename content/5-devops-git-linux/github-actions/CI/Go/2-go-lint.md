
==This does not install dependencies and depends on cached data from previous job, so make sure you write this after the `go test job`==




```yml
  go-lint:
      name: Go Lint
      runs-on: ubuntu-latest
      timeout-minutes: 5

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

          - name: Run golangci-lint
            uses: golangci/golangci-lint-action@v6
            with:
                version: latest
                working-directory: apps/server
                args: --timeout=5m
```

