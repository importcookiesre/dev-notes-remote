
#### step 1 :  create an image of your backend using dockerfile:

```dockerfile
# Use stable LTS version of Node.js
FROM node:20-alpine

# Set working directory
WORKDIR /home/app

# Copy package files first (optimizing caching)
COPY package.json ./

# Install production dependencies only
RUN npm install 


# Copy rest of the application files
COPY . .

# Expose only necessary ports
EXPOSE 8000

# Use production-ready command
CMD ["npm", "run", "start"]

```

**create image command** : docker build 
#### step 2 : create docker-compose file : 

```docker-compose
name: taskify

services:
  backend:
    container_name: taskify-backend
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "8000:8000"
    env_file:
      - .env
    depends_on:
      - mongo
    networks:
      - taskify-network
    volumes:
      - /home/suraj/Desktop/cohort/assignments/week-4/hard:/home/app # Sync local folder to container

  mongo:
    image: mongo
    container_name: taskify-mongo
    restart: always
    volumes:
      - mongo-taskify-volume:/data/db
    ports:
      - "27017:27017"
    networks:
      - taskify-network
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: password

  mongo-express:
    image: mongo-express
    container_name: taskify-mongo-express
    restart: always
    depends_on:
      - mongo
    ports:
      - "8081:8081"
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: admin
      ME_CONFIG_MONGODB_ADMINPASSWORD: password
      ME_CONFIG_MONGODB_URL: mongodb://admin:password@mongo:27017/?authSource=admin
      ME_CONFIG_BASICAUTH: false
    networks:
      - taskify-network

volumes:
  mongo-taskify-volume:

networks:
  taskify-network:
    driver: bridge
```


**see backend logs** : `docker logs -f taskify-backend`

**connect to the container terminal**
```bash
docker exec -it mongodb mongosh -u surajgoraicse -p surajgoraicse --authenticationDatabase admin
```

#### .dockerignore

- used when creating a docker image.
A **`.dockerignore`** file is used to **exclude files and directories** from being copied into a Docker image during the build process. This helps in:

##### **1️ Reducing Image Size**

- Prevents unnecessary files (like `node_modules`, `.git`, or logs) from bloating the image.
##### **2️ Improving Build Performance**

- Excluding large or frequently changing files **speeds up builds** by avoiding unnecessary file copies.
##### **3️ Avoiding Security Risks**

- Prevents sensitive files (`.env`, API keys, SSH keys) from being included in the Docker imag
