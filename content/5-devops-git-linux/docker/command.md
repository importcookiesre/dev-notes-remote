### System commands


| Command                                 | Description            |
| --------------------------------------- | ---------------------- |
| systemctl --user restart docker-desktop | Restart docker desktop |
| `systemctl --user start docker-desktop` | Start docker desktop   |
| `systemctl --user stop docker-desktop`  | Stop docker desktop    |




### **1️⃣ Basic Docker Commands**

| Command            | Description                            |
| ------------------ | -------------------------------------- |
| `docker --version` | Check installed Docker version         |
| `docker info`      | Display system-wide Docker information |
| `docker help`      | Show help for Docker commands          |

---

### **2️⃣ Container Management**

| Command                                              | Description                                            | Example                                    |
| ---------------------------------------------------- | ------------------------------------------------------ | ------------------------------------------ |
| `docker ps`                                          | Lists running containers.                              | `docker ps`                                |
| `docker ps -a`                                       | Lists all containers (running + stopped).              | `docker ps -a`                             |
| `docker run <image>`                                 | Creates and starts a new container.                    | `docker run ubuntu`                        |
| `docker run -d <image>`                              | Runs a container in the background (detached mode).    | `docker run -d nginx`                      |
| `docker run --name <name> <image>`                   | Runs a container with a specific name.                 | `docker run --name myapp nginx`            |
| `docker run -p <host_port>:<container_port> <image>` | Maps ports between the host and the container.         | `docker run -p 8080:80 nginx`              |
| `docker stop <container>`                            | Stops a running container.                             | `docker stop myapp`                        |
| `docker start <container>`                           | Starts a stopped container.                            | `docker start myapp`                       |
| `docker restart <container>`                         | Restarts a container.                                  | `docker restart myapp`                     |
| `docker kill <container id>`                         | removes a container forcefully.                        |                                            |
| `docker rm <container>`                              | Removes a stopped container.                           | `docker rm myapp`                          |
| `docker rm -f <container>`                           | Force removes a running container.                     | `docker rm -f myapp`                       |
| `docker exec -it <container> <command>`              | Runs a command inside a running container.             | `docker exec -it myapp ls -la`             |
| `docker exec -it <container> bash`                   | Opens an interactive shell inside a running container. | `docker exec -it myapp bash`               |
| `docker logs <container>`                            | Shows logs of a container.                             | `docker logs myapp`                        |
| `docker inspect <container>`                         | Displays detailed information about a container.       | `docker inspect myapp`                     |
| `docker stats`                                       | Shows real-time resource usage of containers.          | `docker stats`                             |
| `docker top <container>`                             | Shows running processes inside a container.            | `docker top myapp`                         |
| `docker attach <container>`                          | Attaches to a running container’s console.             | `docker attach myapp`                      |
| `docker cp <container>:<path> <host_path>`           | Copies files from a container to the host.             | `docker cp myapp:/app/data.txt ./data.txt` |
| `docker update --memory <limit> <container>`         | Updates resource limits for a running container.       | `docker update --memory 512m myapp`        |
| `docker commit <container> <new_image>`              | Creates a new image from a container’s state.          | `docker commit myapp my_custom_image`      |
| docker rm -f $(docker ps -aq)<br>                    | remove all containers                                  |                                            |
| `docker rename <oldname > <newname>`                 | renames a container                                    | docker rename my-linux my-linux-container  |
| `docker exec -it <container_id> bash`                | Open an interactive shell inside a running container.  |                                            |

#### docker run 

	- syntax : docker run <options> <image> <command>
	- docker run -it ubuntu === docker run -it ubuntu /bin/bash
	- /bin/bash command is the default command specified by ubuntu image.
	- we can also do : docker run -it ubuntu ls  

---

### **3️⃣ Image Management**

| Command                                               | Description                                  |
| ----------------------------------------------------- | -------------------------------------------- |
| `docker images`                                       | List available Docker images                 |
| `docker pull ubuntu`                                  | Pull an image from Docker Hub                |
| `docker rmi <image_id>`                               | Remove a Docker image                        |
| docker rmi -f $(docker images -aq)<br>                | delete all images                            |
| `docker build -t my-app .`                            | Build an image from a Dockerfile             |
| `docker tag my-app myrepo/my-app:v1`                  | Tag an image for pushing to a registry       |
| `docker push myrepo/my-app:v1`                        | Push an i mage to a Docker registry          |
| docker inspect <image>                                | inspect image                                |
| `docker build -t <image-name> -f <dockerfile-path> .` | docker build -t tsc-old -f  dockerfile.old . |

---

### **4️⃣ Container Inspection**

| Command                         | Description                                         |
| ------------------------------- | --------------------------------------------------- |
| `docker logs <container_id>`    | View logs of a running container                    |
| `docker inspect <container_id>` | Get detailed information about a container          |
| `docker stats`                  | Show real-time resource usage of running containers |
| `docker top <container_id>`     | Display processes running inside a container        |
| `docker events`                 | Show real-time events from Docker                   |

---

### **5️⃣ Container Interaction**

| Command                                     | Description                                          |
| ------------------------------------------- | ---------------------------------------------------- |
| `docker exec -it <container_id> bash`       | Open an interactive shell inside a running container |
| `docker attach <container_id>`              | Attach to a running container                        |
| `docker cp <container_id>:/path/to/file ./` | Copy files from a container to the host              |

---

### **6️⃣ Volume Management**

| Command                           | Description                |
| --------------------------------- | -------------------------- |
| `docker volume create my-volume`  | Create a new volume        |
| `docker volume ls`                | List all volumes           |
| `docker volume inspect my-volume` | Get details about a volume |
| `docker volume rm my-volume`      | Remove a volume            |

---


### **8️⃣ Docker Compose**

| Command                | Description                                   |
| ---------------------- | --------------------------------------------- |
| `docker-compose up -d` | Start all services in `docker-compose.yml`    |
| `docker-compose down`  | Stop and remove all services                  |
| `docker-compose ps`    | List services defined in `docker-compose.yml` |

---

### **9️⃣ Cleanup & Maintenance**

| Command                  | Description                                                |
| ------------------------ | ---------------------------------------------------------- |
| `docker system prune -a` | Remove all stopped containers, unused images, and networks |
| `docker container prune` | Remove all stopped containers                              |
| `docker image prune`     | Remove unused images                                       |
| `docker volume prune`    | Remove unused volumes                                      |

---
