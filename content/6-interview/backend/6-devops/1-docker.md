

1. **What is a Container ?**
	1. A container is a isolated environment that packages our application code along with all its dependencies like  an OS, system tools, libraries, and configuration files into a single deployable unit.


2. **What problems Do Container solves ?**
	1. It solves the classical : "It works on my machine" problem.
		1. When we build a Docker image, we package all the dependencies with a base OS into a single artifact. 
		2. Now we simply run this image across multiple environement like dev, prod, test and it works the same.

	2. Process isolation :
		1. We can run multiple versions of the same software like postgres in a single machine which can server different application.
		2. Example : we have two services : one uses postgres 15 and other uses postgres 18, using docker container we can run both at the same time in an isolated environment.

	3. Slow scaling and deployment :
		1. Our backend gets hit by a massive traffic spike. We can use docker to scale by creating new instances of the server.
		2. A new instance of your Go API can be pulled and started in milliseconds, allowing orchestration tools (like Kubernetes) to auto-scale our system almost instantly in response to traffic.


4. **Container vs Virtual machines**
	Both are used to isolate applications and their dependencies but they achieve this isolation at entirely different technology.
	

