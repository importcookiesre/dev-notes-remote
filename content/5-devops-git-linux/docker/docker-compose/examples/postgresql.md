


### docker-compose.yml

```python
name: postgres
services:
  postgres:
    image: postgres
    container_name: postgres
    environment:
      POSTGRES_USER: surajgoraicse
      POSTGRES_PASSWORD: surajgoraicse
      POSTGRES_DB: test_db
    ports:
      - "5432:5432"
    volumes:
      - postgres:/var/lib/postgresql/data
    restart: always
    networks:
      - my-network

  pgadmin:
    image: dpage/pgadmin4
    container_name: pgadmin
    environment:
      PGADMIN_DEFAULT_EMAIL: surajgoraicse@gmail.com
      PGADMIN_DEFAULT_PASSWORD: surajgoraicse
    ports:
      - "8080:80"
    depends_on:
      - postgres
    restart: always
    networks:
      - my-network

volumes:
  postgres:

networks:
  my-network:
    driver: bridge
```


**start docker :**
```
docker-compose up -d
```

**start postgres shell in terminal**
```
docker exec -it postgres psql -U surajgoraicse -d postgres
```


```
postgresql://surajgoraicse:surajgoraicse@postgres:5432/portfolio?schema=public
postgresql://surajgoraicse:surajgoraicse@postgres:5432/<here>?schema=public
```

Use `localhost` when running locally from your machine  
Use `postgres` if querying from another Docker container