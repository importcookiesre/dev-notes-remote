#### What is a docker ?

- Docker is an open source containerization platform by which we can pack our application and all its dependencies into a standardised unit called a container. 

#### How Docker process a run request ?

docker run ubuntu

- The Docker client contacted the Docker daemon.
- The Docker daemon pulls the "ubuntu" image from the Docker Hub.
- The Docker daemon creates a new container from that image which runs the executable that produces output.
- The Docker daemon streamed that output to the Docker client, which is sent to the terminal


#### Key Components of Docker

The following are the some of the key components of Docker:

- ***Docker Engine:***  It is a core part of docker, that handles the creation and management of containers.
- ***Docker Image:** It is a read-only template/configuration that is used for creating containers, containing the application code and dependencies.
- ***Docker Hub:*** It is a cloud based repository that is used for finding and sharing the container images.
- ***Dockerfile:*** It is a script that containing instructions to build a docker image.
- ***Docker Registry*** : It is a storage distribution system for docker images, where you can store the images in both public and private modes.


#### Docker image:

- Docker image is a set of instructions / configurations used to create docker containers.
- Docker Image is an executable package of software that includes everything needed to run an application.

#### Docker container

- Docker container is a runtime instance of an image.




#### Docker Compose

- Docker Compose execute a YAML-based multi-container application. 
- The YAML file consists of all configurations needed to deploy containers Docker Compose , which is integrated with Docker Swarm , and provides directions for building and deploying containers. 
- With Docker Compose, each container is constructed to run on a single host.


