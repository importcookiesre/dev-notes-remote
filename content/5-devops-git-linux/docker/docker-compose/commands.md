

**up docker-compose**
```docker-compose 
	docker-compose -f src/docker-compose-mongo.yml up -d
```

**start mongo shell**
```
docker exec -it mongodb mongosh -u surajgoraicse -p surajgoraicse --authenticationDatabase admin
```

## 🧱 Basic Commands

| Command                  | Description                                                              |
| ------------------------ | ------------------------------------------------------------------------ |
| `docker-compose up`      | Builds, (re)creates, starts services in the foreground.                  |
| `docker-compose up -d`   | Same as above, but runs in **detached** mode (in the background).        |
| `docker-compose down`    | Stops and removes containers, networks, and volumes defined in the file. |
| `docker-compose restart` | Restarts all services.                                                   |
| `docker-compose stop`    | Stops running containers but doesn't remove them.                        |
| `docker-compose start`   | Starts services that were previously stopped.                            |
|                          |                                                                          |
|                          |                                                                          |

| Command                                             | Description                                                                  |
| --------------------------------------------------- | ---------------------------------------------------------------------------- |
| `docker-compose -f <path>/docker-compose.yml up -d` | Use a specific Compose file.                                                 |
| `docker-compose -p <project_name> up -d`            | Assigns a name to your Compose project (affects container & network naming). |


---

## 🔍 Inspection & Debugging

|Command|Description|
|---|---|
|`docker-compose ps`|Shows status of services.|
|`docker-compose logs`|Shows logs from all containers.|
|`docker-compose logs -f`|**Follow** log output (real-time tailing).|
|`docker-compose logs <service>`|Logs for a specific service.|
|`docker-compose top`|Displays the running processes inside your containers.|

---

## 🛠️ Build & Manage Services

|Command|Description|
|---|---|
|`docker-compose build`|Builds or rebuilds the services.|
|`docker-compose build --no-cache`|Rebuilds without using cache.|
|`docker-compose pull`|Pulls service images defined in the file.|
|`docker-compose push`|Pushes built images to a registry.|

---

## 🧹 Cleanup & Remove

|Command|Description|
|---|---|
|`docker-compose down -v`|Also removes volumes created by services.|
|`docker-compose rm`|Removes stopped service containers.|
|`docker-compose down --rmi all`|Removes all images used by any service.|
|`docker-compose down --volumes --remove-orphans`|Removes volumes and orphan containers.|

---

## 🧪 Running Commands Inside Services

|Command|Description|
|---|---|
|`docker-compose exec <service> <command>`|Run a command in a **running container**. Ex: `docker-compose exec app bash`|
|`docker-compose run <service> <command>`|Starts a **new container** to run a one-off command. Ex: `docker-compose run app npm install`|

> `exec` = runs in running container  
> `run` = spins up new one

---
