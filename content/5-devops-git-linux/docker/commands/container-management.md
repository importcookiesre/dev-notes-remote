### **Container Management**

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

---

### Flags

#### Docker run : 

| Flag                                  | Description                                                            | Example                                       |
| ------------------------------------- | ---------------------------------------------------------------------- | --------------------------------------------- |
| `-d`                                  | Runs the container in detached mode (background)                       | `docker run -d nginx`                         |
| `-p <host>:<container>`               | Maps a container port to a host port                                   | `docker run -p 8080:80 nginx`                 |
| `-it`                                 | Runs the container interactively with a terminal                       | `docker run -it ubuntu bash`                  |
| `--name <name>`                       | Assigns a custom name to the container                                 | `docker run --name myapp nginx`               |
| `-e <KEY>=<VALUE>`                    | Sets environment variables                                             | `docker run -e NODE_ENV=production node`      |
| `--rm`                                | Automatically removes the container after it stops                     | `docker run --rm alpine ls`                   |
| `-v <host_path>:<container_path>`     | Mounts a volume (binds a host directory to a container directory)      | `docker run -v $(pwd):/app node`              |
| `--network <network>`                 | Connects the container to a specific network                           | `docker run --network my_network nginx`       |
| `-w <path>`                           | Sets the working directory inside the container                        | `docker run -w /app node npm install`         |
| `-u <user>`                           | Runs the container as a specific user                                  | `docker run -u 1000:1000 ubuntu`              |
| `--entrypoint <command>`              | Overrides the default `ENTRYPOINT` defined in the Dockerfile           | `docker run --entrypoint /bin/sh nginx`       |
| `--log-driver <driver>`               | Specifies a logging driver for container logs                          | `docker run --log-driver json-file nginx`     |
| `--cpu-shares <value>`                | Assigns relative CPU weight for container processes                    | `docker run --cpu-shares 512 nginx`           |
| `--memory <value>`                    | Limits container memory usage                                          | `docker run --memory=512m nginx`              |
| `--restart <policy>`                  | Sets a restart policy (`no`, `always`, `unless-stopped`, `on-failure`) | `docker run --restart always nginx`           |
| `--cap-add <capability>`              | Grants additional Linux capabilities to the container                  | `docker run --cap-add NET_ADMIN ubuntu`       |
| `--privileged`                        | Gives the container full host privileges                               | `docker run --privileged ubuntu`              |
| `--device <host_dev>:<container_dev>` | Allows access to a device from the host                                | `docker run --device=/dev/video0:/dev/video0` |


#### docker run

	- syntax : docker run <options> <image> <command>
	- docker run -it ubuntu === docker run -it ubuntu /bin/bash
	- /bin/bash command is the default command specified by ubuntu image.
	- we can also do : docker run -it ubuntu ls  

exposing port and env var. 

	- docker run -it -P  -e  port=3000  -e  uri=link  node-project
	- docker run -it -P  --env-file=./.env.ts  node-project 
		- here env file is not needed to pass to the image in dockerfile.
	- we can also provide environmental variables in the dockerfile




