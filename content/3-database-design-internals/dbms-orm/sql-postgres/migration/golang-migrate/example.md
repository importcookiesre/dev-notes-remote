

We can run either using docker or by installing it locally.
```go
migrate:
image: migrate/migrate
volumes:
  - ./db/migrations:/migrations
entrypoint: ["migrate"]
depends_on:
  postgres:
	condition: service_healthy
```


make file : 
```go
MIGRATE := docker compose run --rm migrate
DB_URL := postgres://app:app@postgres:5432/app?sslmode=disable
MIGRATIONS := /migrations

migrate-create:
	@read -p "name: " name; \
	$(MIGRATE) create -ext sql -dir $(MIGRATIONS) -seq "$$name"

migrate-up:
	$(MIGRATE) -path $(MIGRATIONS) -database "$(DB_URL)" up

migrate-down:
	$(MIGRATE) -path $(MIGRATIONS) -database "$(DB_URL)" down 1

migrate-force:
	@read -p "version: " v; \
	$(MIGRATE) -path $(MIGRATIONS) -database "$(DB_URL)" force $$v

migrate-version:
	$(MIGRATE) -path $(MIGRATIONS) -database "$(DB_URL)" version
```

better migrate create command.
```go
migrate-create:
	@if [ -z "$(name)" ]; then \
	  echo "Please provide 'name' var, e.g. 'make migrate-create name=add_thing'"; exit 1; \
	fi
	docker compose run --rm migrate create -ext sql -dir /migrations -seq $(name)
```


