

```yml
# SQLC Code Generation Validation
  sqlc-validate:
      name: SQLC Validation
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

          - name: Install sqlc
            run: go install github.com/sqlc-dev/sqlc/cmd/sqlc@latest

          - name: Generate SQLC code
            working-directory: apps/server
            run: sqlc generate

          - name: Check for uncommitted changes
            working-directory: apps/server
            run: |
                if [ -n "$(git status --porcelain internal/db/sqlc)" ]; then
                  echo "Error: SQLC generated code is out of sync. Please run 'sqlc generate' and commit the changes."
                  git diff internal/db/sqlc
                  exit 1
                fi
```