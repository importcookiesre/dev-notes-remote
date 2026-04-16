
A **Dockerfile** is a script-like text file that contains a set of instructions to build a **Docker image**. It automates the process of creating containerized applications by specifying the operating system, dependencies, configurations, and commands required for the application to run.

### **🚀 Common Dockerfile Commands**

| **Command**   | **Description**                                                          | **Example**                                 |
| ------------- | ------------------------------------------------------------------------ | ------------------------------------------- |
| `FROM`        | Specifies the base image for the container                               | `FROM ubuntu:latest`                        |
| `RUN`         | Executes commands during image build                                     | `RUN apt update && apt install -y neovim`   |
| `CMD`         | Defines the default command to run when the container starts             | `CMD ["node", "server.js"]`                 |
| `ENTRYPOINT`  | Similar to `CMD`, but doesn’t get overridden by arguments                | `ENTRYPOINT ["python3", "app.py"]`          |
| `COPY`        | Copies files from the host system into the container                     | `COPY . /app`                               |
| `ADD`         | Similar to `COPY`, but supports remote URLs and archive extraction       | `ADD https://example.com/file.tar.gz /app/` |
| `WORKDIR`     | Sets the working directory for subsequent commands                       | `WORKDIR /app`                              |
| `ENV`         | Sets environment variables inside the container                          | `ENV NODE_ENV=production`                   |
| `ARG`         | Defines build-time variables                                             | `ARG VERSION=1.0`                           |
| `EXPOSE`      | Documents the ports the container listens on (doesn't publish them)      | `EXPOSE 3000`                               |
| `VOLUME`      | Creates a mount point for persistent storage                             | `VOLUME /data`                              |
| `USER`        | Specifies which user should run the container process                    | `USER node`                                 |
| `LABEL`       | Adds metadata to the image                                               | `LABEL maintainer="yourname@example.com"`   |
| `HEALTHCHECK` | Defines a command to check if the container is healthy                   | `HEALTHCHECK CMD curl -f http://localhost   |
| `ONBUILD`     | Executes commands when the image is used as a base in another Dockerfile | `ONBUILD COPY . /app`                       |
| `SHELL`       | Changes the default shell for `RUN` commands                             | `SHELL ["/bin/bash", "-c"]`                 |
| `STOPSIGNAL`  | Defines the system call signal to stop the container                     | `STOPSIGNAL SIGTERM`                        |

#### dockerfile


build command : `docker build -t <tag> <location>`
docker build -t my-app .


1. step 1
```
FROM ubuntu

RUN apt -y update
RUN apt install -y curl
RUN curl -sL https://deb.nodesource.com/setup_22.x -o /tmp/nodesource_setup.sh
RUN bash /tmp/nodesource_setup.sh
RUN apt install -y nodejs

COPY index.js /home/app/index.js
COPY package.json /home/app/package.json
COPY package-lock.json /home/app/package-lock.json

ENV port=8000

WORKDIR /home/app
RUN npm install
```
the problems here is : 
- No port binding. 
- The image is heavy (> 500mb ).
- Not efficinetly caching

2. step 2 
```
FROM node:22.14-alpine3.21

COPY index.js /home/app/index.js
COPY package.json /home/app/package.json
COPY package-lock.json /home/app/package-lock.json

WORKDIR /home/app

RUN npm install
```
the problem here is : 
	- no port binding
	- not efficiently caching

3. step 2
```
FROM node:22.14-alpine3.21

WORKDIR /home/app

COPY package*.json .
RUN npm install
COPY index.js index.js

CMD [ "npm", "run", "dev" ]
```

```dockerfile
# base image
FROM ubuntu

RUN apt-get -y update
RUN apt-get -y install curl
RUN curl -sL https://deb.nodesource.com/setup_20.x -o /tmp/nodesource_setup.sh
RUN bash /tmp/nodesource_setup.sh
RUN apt install -y nodejs


# copying the source code to docker image
COPY index.js /home/app/index.js
COPY package.json /home/app/package.json

WORKDIR /home/app

RUN npm install
```



#### Port maping

`docker run -it -p <host-port>:<container-port> `
`docker run -it -p 3000:8000 -p 4000:4000 my-app`


**automatic port maping**
- docker build -t -P my-app .
```
FROM node:22.14-alpine3.21

WORKDIR /home/app

COPY package*.json .
RUN npm install
COPY index.js index.js

CMD [ "npm", "run", "dev" ]

EXPOSE 4000
```
![[Pasted image 20250322050648.png]]

```
EXPOSE 3000
EXPOSE 3000-3010
EXPOSE 3000 3001 3002
```



#### Multistage Builds

A **multi-stage build** in Docker is a technique that allows you to use multiple `FROM` statements in a single `Dockerfile`, creating different **stages** of the build process. This helps in **reducing image size**, improving **security**, and ensuring **cleaner builds**.

- After each step the previous stage gets deleted and the final stage only contains the necessary data.
- 

```
# Stage 1: Base Image
FROM node:20.19.0-alpine3.21 AS base

# Stage 2: Build Stage
FROM base AS builder
WORKDIR /app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm install 

# Copy source code and compile TypeScript
COPY src/ src/
RUN npm run build

# Stage 3: Production Ready Image
FROM base AS runner
WORKDIR /app

# Copy only the necessary files from the build stage
COPY --from=builder /app/dist dist/
COPY --from=builder /app/package*.json ./

# Install only production dependencies
RUN npm install --omit=dev

# Start the application
CMD ["node", "dist/index.js"]

```

#### Secure user management :

- **Creates a non-root user (`nodejs`)** to improve security.
- Ensures that the application does **not run as root**.
- Follows **best security practices** to minimize attack risks.


```
# Secure user management
RUN addgroup --system --gid 1001 nodejs
RUN adduser --system -u 1001 -G nodejs nodejs

# Run the application as a non-root user
USER nodejs

```