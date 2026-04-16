
### Core Components

1. **Services**: These are the containers you want to run (e.g., a Postgres database, a Redis cache, or your app). Each service is defined in the YAML file and maps to a container.
2. **Networks**: Compose sets up a default network so your services can talk to each other (e.g., your app can reach db by its service name).
3. **Volumes**: These persist data outside containers, like you saw with Docker volumes earlier.



```python
name: e-commerce

services:
  backend:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: backend
    ports:
      - "8000:8000"

  db:
    image: postgres:16
    container_name: postgres
    environment:
      POSTGRES_PASSWORD: postgres
      POSTGRES_USER: postgres
      POSTGRES_DB: postgres
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    container_name: redis
    volumes:
      - redis_data:/data

volumes:
  postgres_data:
  redis_data:
```

#### commands : 


**1. Start the compose :**
docker-compose up -d

**2. rebuild the compose container when it is running :** 
```bash
docker-compose up -d --build  
docker-compose build mongo    
%% build only one service %%
```

**3. Stop the compose :**
```bash
docker-compose down
```


#### volume : 


#### app in docker :

####  network in docker compose


