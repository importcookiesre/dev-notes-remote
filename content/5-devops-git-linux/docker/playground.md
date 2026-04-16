





## 1. ubuntu : 

1. create a container and run in terminal : 
```python
docker run -it ubuntu 
docker run -it --rm ubuntu # remove when stopped (temp containers) 


# volume, port 
docker run -p 8080:8080 -p 8081:8081 -v my_volume:/data --name my_ubuntu -it ubuntu

# local mount
docker run -p 8080:8080 -p 8081:8081 -v /home/suraj/docker_data:/data --name my_ubuntu -it ubuntu

```

2. start a container stopped container:
```python
docker start constainer_name

# open in intercative mode
docker start -i my_ubuntu

```

3. Access the terminal of a running container:
```python
docker exec [options] <container_name_or_id> <command>

# access terminal of running ubuntu, here /bin/bash is required as during start or run command it automatically pass the /bin/bash command, therefore we have to pass it explicitly here.
docker exec -it my_ubuntu /bin/bash


# pass more command
 docker exec -it postgres psql -U surajgoraicse -d postgres
```

4. logs:
```python

# see logs of the container
docker logs my_ubunut
```

5. docker compose:


```
```


## run redis : 

## postgres :

### 1. cli : 

- this wil create a volume if not exist.

```bash
docker run -d \
  --name postgres \
  -e POSTGRES_USER=surajgoraicse \
  -e POSTGRES_PASSWORD=surajgoraicse \
  -e POSTGRES_DB=mydb \
  -p 5432:5432 \
  -v pgdata:/var/lib/postgresql \
  postgres
```

connect with psql :
- here mydb is the name of a existing db.
```bash
docker exec -it postgres psql -U surajgoraicse -d mydb
```

connection url:
```bash
postgresql://surajgoraicse:surajgoraicse@localhost:5432/mydb  # from localhost
postgresql://surajgoraicse:surajgoraicse@postgres:5432/mydb # from another container
```
## run mongodb :


## run mySql : 





## 3. make a docker service (image)


### a. nodejs backend




## publish your image  to docker hub.


# deployment

