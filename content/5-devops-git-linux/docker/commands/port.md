
## port binding : 

- use to publish a port of a containers port to the host machine's port.
- cannot bind port after creation of container. It should be mapped during the creation of the container with `docker run` command.

- `host_port:container_port` => 8080:8080

ex:
```bash
docker run -p 8080:8080 my_image
docker run -p 8080:8080 -p 8081:8081 --name my_container my_image

```


**port publish vs expose**

| Feature        | EXPOSE                                                     | PUBLISH (-p)                                                                              |
| -------------- | ---------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| **Location**   | Dockerfile (static)                                        | `docker run` command (runtime)                                                            |
| **Purpose**    | Documentation & inter-container networking (communication) | Creates a host-to-container port mapping. Communicate between host machine and conatiner. |
| **Access**     | Not accessible from host/external network by default       | Accessible from host/external network                                                     |
| **Mandatory?** | No, but good practice                                      | Yes, if external access is needed                                                         |


