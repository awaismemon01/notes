# Docker notes

Docker is a container management service.
Platform for virtualization with multiple OS running on same host.

Docker containers run on top of host's OS.
	improves efficiency and security.
	can run more containers with same infrastructure than VMs.
	
VMs are resource heavy.

Docker is client-server type of application
Docker daemon called: dockerd is the Docker engine which represents the server.


Docker images are the “source code” for our containers; we use them to build containers
images can contain softwares pre installed
images are portable

Docker stores the images we build in registries
Dockerfile has two types of registries public and private registries
Public registries are stores on Docker Hub
can also store private images

Containers are the organizational units and one of the Docker basics concept. When we build an image and start running it; we are running in a container.

Download and Install Docker Desktop

For windows install Windows Subsystem for Linux

some docker commands
	```
  docker images
		list all images
  ```
	docker ps
		list of running containers

	docker ps -a
		list of all containers
	
	docker pull <name of image>
		used to pull the specified image from public registry - Docker hub
		
		
	docker run <name of image>
		used to create a container with the specified image and run it

	docker rmi ImageID
		remove image
		
	docker inspect <name of image/container>
		see details of image or container
		
	docker stop ContainerID
		stop the specified container
		
	docker rm ContainerID
		remove the specified container
		
	docker <pause/unpause> ContainerID
		pause/unpause the specified container
		
	docker kill ContainerID
		kill the specified container
		
		
	docker run --name -p 8080:80
	
		-p is used to map the container port to host port
		{HostPort}:{ContainerPort}
```    
## Dockerfile

	FROM 		Initialize a new build stage and sets the Base Image for subsequent instructions.
		
		
	ARG		Define a variable that users can pass at build-time to the builder with the docker build.
		
	RUN		Execute any commands in a new layer on top of the current image and commit the results.
		
		
	COPY		Copy new files or directories from and adds them to the filesystem of the container at the path.
		
	EXPOSE		Inform Docker that the container listens on the specified network ports at runtime.

	ENTRYPOINT	Allow you to configure a container that will run as an executable.
		
	WORKDIR 	sets the directory that the container starts in. Its main purpose is to set the working directory for all future Dockerfile commands.
		
		
## Docker Compose
	For orchestrating multiple containers
	
	command 		docker-compose up 			
      start based on the compose file in current directory (i.e. docker-compose.yml)
			
			
##	Docker Compose File Structure
	
	version: '3'
	services:
	  <service-name>:
	  
		# Path to dockerfile.
		# '.' represents the current directory in which
		# docker-compose.yml is present.
		build: .

		# Mapping of container port to host
		ports:
		  - "5000:5000"

		# Link database container to app container 
		# for reachability.
		links:
		  - "database:backenddb"
		
	  database:

		# image to fetch from docker hub
		image: postgres:alpine

		# Environment variables for startup script
		# container will use these variables
		# to start the container with these define variables. 
		environment:
		  - POSTGRES_DB=crux_local_4_refector
		  - POSTGRES_USER=postgres
		  - POSTGRES_PASSWORD=PassPost
