



```yml
# Swagger Documentation Validation
  swagger-validate:
      name: Swagger Validation
      runs-on: ubuntu-latest
      timeout-minutes: 5

      steps:
          - name: Checkout code
            uses: actions/checkout@v4

          # ---- Go setup (includes Go module + build cache) ----
          - name: Set up Go
            uses: actions/setup-go@v5
            with:
                go-version: ${{ env.GO_VERSION }}
                cache-dependency-path: apps/server/go.sum

          # ---- pnpm + Node setup (same pattern as frontend) ----
          - name: Set up pnpm
            uses: pnpm/action-setup@v4
            with:
                version: 10.11.0

          - name: Set up Node.js
            uses: actions/setup-node@v4
            with:
                node-version: ${{ env.NODE_VERSION }}

          - name: Get pnpm store directory
            id: pnpm-cache
            shell: bash
            run: echo "STORE_PATH=$(pnpm store path --silent)" >> $GITHUB_OUTPUT

          - name: Cache pnpm store
            uses: actions/cache@v4
            with:
                path: ${{ steps.pnpm-cache.outputs.STORE_PATH }}
                key: ${{ runner.os }}-pnpm-store-${{ hashFiles('apps/server/pnpm-lock.yaml', 'apps/server/package.json') }}
                restore-keys: |
                    ${{ runner.os }}-pnpm-store-

          - name: Install Node dependencies
            working-directory: apps/server
            run: pnpm install --frozen-lockfile

          # ---- Swagger generation ----
          - name: Install swag
            run: go install github.com/swaggo/swag/cmd/swag@latest

          - name: Generate Swagger docs
            working-directory: apps/server
            run: |
                swag init \
                  -o ./swagger \
                  --parseDependency \
                  --parseInternal \
                  -g cmd/api-server/main.go

          - name: Format Swagger docs (Prettier)
            run: |
                cd apps/server
                npx prettier --write swagger/swagger.json
                npx prettier --write swagger/swagger.yaml

          # ---- Validation ----
          - name: Check for uncommitted Swagger changes
            working-directory: apps/server
            run: |
                if [ -n "$(git status --porcelain swagger)" ]; then
                  echo "❌ Swagger documentation is out of sync."
                  echo "👉 Run 'make swagger' (or swag + prettier) and commit the changes."
                  git diff swagger
                  exit 1
                fi
```