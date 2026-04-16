
#### How communication is done ?

There are two ways in which communication can be done by the containers : 
1. Exposing Ports (Host-to-Container or External-to-Container)
2. User-Defined Network (Container-to-Container):


| Command                                               | Description                           |
| ----------------------------------------------------- | ------------------------------------- |
| `docker network ls`                                   | List all networks                     |
| `docker network create my-network`                    | Create a new Docker network           |
| `docker network inspect my-network`                   | Get details about a network           |
| `docker network connect my-network <container_id>`    | Connect a container to a network      |
| `docker network disconnect my-network <container_id>` | Disconnect a container from a network |

Containers have networking enabled by default, and they can make outgoing connections. A container has no information about what kind of network it's attached to, or whether their peers are also Docker workloads or not. A container only sees a network interface with an IP address, a gateway, a routing table, DNS services, and other networking details. That is, unless the container uses the `none` network driver. 


| Driver    | Description                                                              |
| --------- | ------------------------------------------------------------------------ |
| `bridge`  | The default network driver.                                              |
| `host`    | Remove network isolation between the container and the Docker host.      |
| `none`    | Completely isolate a container from the host and other containers.       |
| `overlay` | Overlay networks connect multiple Docker daemons together.               |
| `ipvlan`  | IPvlan networks provide full control over both IPv4 and IPv6 addressing. |
| `macvlan` | Assign a MAC address to a container.                                     |
#### Bridge (default) : 

A bridge network in Docker is a way to connect containers so they can talk to each other (or the host) while keeping them isolated from the outside world by default. 

Think of it like a virtual switch or a private LAN inside your machine. It’s built on a Linux bridge (a kernel feature) that Docker creates, usually named docker0. Containers plugged into this network get their own IP addresses (typically 172.17.x.x range) and can communicate internally, but they’re hidden behind the host’s IP from external networks unless you expose ports.

![[Pasted image 20250322114240.png]]
- A machine has **network drivers** which connects the machine to the network router.
- This machine has a network ip, ... 
- Now what docker does here is, when we create a container it creates a virtual network drivers (here it is bridge) and then it connect to the network router.
- Therefore each container have a unique ip. 

**Properties**

- **Use Case**: When multiple containers need to communicate on the same host. 
	- eg : nodejs app need to communicate with redis , postgres , etc and it does not need to expose those ports which may cause security issues. 
- **Behavior**: Containers inside the same bridge network can talk to each other using **container names**.
- **Default Network**: When you run a container using `docker run`, it gets assigned to the `bridge` network unless specified otherwise.

![[Pasted image 20250322114212.png]]


#### User-defined network :

##### How it works : 

1. Create a network : 
```
docker network create myapp-net
```

2. Attach containeres to the network : 
```bash

docker run -d --network myapp-net --name web nginx
docker run -d --network myapp-net --name db mysql
```
- web and db are on myapp-net.
- web can hit db at mysql:3306 without extra config.
- Nothing on the default docker0 network can reach them unless you bridge networks explicitly.
#####  Properties : 

**Isolation**: Containers on mynetwork are isolated from:

- The default bridge network (docker0).
- Other user-defined networks.
- The outside world (unless you map ports with -p).



##### Types of User-Defined Networks:

1. **Bridge**: Default type for single-host setups (like above). Good for local dev or simple apps.
2. **Overlay**: For multi-host (e.g., Docker Swarm). Containers on different machines talk seamlessly.
3. **Macvlan**: Ties containers to physical network interfaces—rare for beginners.



### Host network : 


## Learn about other network 