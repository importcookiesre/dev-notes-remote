
## Containers achieve isolation using three native Linux kernel features:

1. **Namespaces:** This provides _isolation_. It tricks the process into thinking it has its own dedicated system resources. A process in a container has its own Process ID (PID) tree, network interfaces (NET), and mount points (MNT). If a Go app crashes inside a container, it only sees its own tiny ecosystem.
    
2. **Control Groups (cgroups):** This provides _resource management_. It allows you to strictly limit how much CPU, memory, or disk I/O a specific container can use. You can tell Docker, "Only let this Postgres container use 512MB of RAM."
    
3. **Union File Systems (UnionFS):** This provides _layered, lightweight storage_. A Docker image is built in read-only layers. When you start a container from an image, Docker adds a thin, writable layer on top. This is why you can spin up 10 identical containers in milliseconds—they all share the same underlying read-only base files.

#### Hypervisor : 

- Hypervisor is used to create simulated machine (cpu , ram ...) using real machine. eg: virtual machine
- Like aws does not give a real machine but a simulated machine / a layer of a machine.
- Now docker is an **engine** similar to **hypervisor** but much lighter.




#### publish your image to docker hub


##### method 1 : 
- build an image with the same name as a docker hub repo
	- docker build -t same-name-as-repo . 
- and then push the image

##### method 2 :

- make a docker tag of the local image and then push it
```
docker tag my-app surajgoraicse/node-application
docker push surajgoraicse/node-application
```


##### versioning :

```
docker tag my-app surajgoraicse/node-application:v1
docker push surajgoraicse/node-application:v1
```



#### setup mongo mongo-express docker container

1. create a network 
2. create mongo container
3. create mongo express container


#### docker compose 














### Lectures :: 

- command to handle container, images.
- docker file
	- port binding
	- efficient caching
- publish docker images to docker hub.
	- 1. build with the same name and publish
	- 2. create a tag of the local image with the same name as the repo
		- and then push
		- 

