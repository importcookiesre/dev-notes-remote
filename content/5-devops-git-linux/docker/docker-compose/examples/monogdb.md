

```
name: mern

services:
  mongo:
    image: mongo
    container_name: mongodb
    restart: always
    volumes:
      - mongodb:/data/db
    ports:
      - "27017:27017"
    networks:
      - my-network
    environment:
      MONGO_INITDB_ROOT_USERNAME: surajgoraicse
      MONGO_INITDB_ROOT_PASSWORD: surajgoraicse

  mongo-express:
    image: mongo-express
    container_name: mongo-express-cohort
    restart: always
    depends_on:
      - mongo
    ports:
      - "8081:8081"
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: surajgoraicse
      ME_CONFIG_MONGODB_ADMINPASSWORD: surajgoraicse
      ME_CONFIG_MONGODB_URL: mongodb://surajgoraicse:surajgoraicse@mongo:27017/?authSource=admin
      ME_CONFIG_BASICAUTH: false

    networks:
      - my-network

volumes: 
  mongodb:

networks:
  my-network:
    driver: bridge

```



**start the docker-compose:**
```
docker-compose up -d
**start monogo shell**
```

```
docker exec -it mongodb mongosh -u surajgoraicse -p surajgoraicse --authenticationDatabase admin
```