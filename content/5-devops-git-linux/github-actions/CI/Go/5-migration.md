

```yml

# Database Migration Validation
  migration-check:
      name: Migration Check
      runs-on: ubuntu-latest
      timeout-minutes: 5

      services:
          postgres:
              image: postgres:18-alpine
              env:
                  POSTGRES_USER: postgres
                  POSTGRES_PASSWORD: mysecretpassword
                  POSTGRES_DB: techshastra
              options: >-
                  --health-cmd pg_isready
                  --health-interval 10s
                  --health-timeout 5s
                  --health-retries 5
              ports:
                  - 5434:5432

      steps:
          - name: Checkout code
            uses: actions/checkout@v4

          - name: Install PostgreSQL client
            run: |
                sudo apt-get update
                sudo apt-get install -y postgresql-client

          - name: Wait for PostgreSQL
            run: |
                until pg_isready -h localhost -p 5434 -U postgres; do
                  echo "Waiting for PostgreSQL..."
                  sleep 2
                done

          - name: Validate migration syntax (dry-run)
            working-directory: packages/server
            env:
                PGHOST: localhost
                PGPORT: 5434
                PGUSER: postgres
                PGPASSWORD: mysecretpassword
                PGDATABASE: techshastra
            run: |
                # Create schemas
                psql -h localhost -U postgres -d techshastra -c "CREATE SCHEMA IF NOT EXISTS techshastra;"
                psql -h localhost -U postgres -d techshastra -c "CREATE SCHEMA IF NOT EXISTS ory_keto;"

                # Validate migration file syntax using dry-run (transaction rollback)
                psql -h localhost -U postgres -d techshastra -v ON_ERROR_STOP=1 <<'EOF'
                BEGIN;
                \set ON_ERROR_STOP on
                \i schema/001_initial.sql
                ROLLBACK;
                EOF

                echo "Migration syntax validation passed!"
```