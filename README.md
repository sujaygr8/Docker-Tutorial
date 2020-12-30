# Docker-Tutorial
Introductory commands and description

//To practice labs Install Docker or use Play-With-Docker//

Containers are just a process (or a group of processes) running in isolation, which is achieved with Linux namespaces and control groups. Linux namespaces and control groups are features that are built into the Linux kernel. Other than the Linux kernel itself, there is nothing special about containers.

**Run a container

# docker container run -t ubuntu top
You use the docker container run command to run a container with the Ubuntu image by using the top command. The -t flag allocates a pseudo-TTY, which you need for the top command to work correctly.The docker run command first starts a docker pull to download the Ubuntu image onto your host.

**Inspect the container:

# docker container exec
This command allows you to enter a running container's namespaces with a new process.

**To check the container ID 

# docker container ls 
# docker container ps 
# docker container ps -a 
# docker container ps -q 

The ls and ps flag list all the containers. -a shows all the containers, -q is used to show the running containers.

**Use that container ID to run bash inside that container by using the docker container exec command. Because you are using bash and want to interact with this container from your terminal, use the -it flag to run using interactive mode while allocating a psuedo-terminal:

# docker container exec -it b3ad2a23fab3 bash 

WE CAN RUN MULTIPLE CONTAINERS AT ONCE

**Run multiple containers

# docker container run --detach --publish 8080:80 --name nginx nginx                 (this command will run a nginx server)
# docker container run --detach --publish 8081:27017 --name mongo mongo:3.4          (this command will run a mongodb server)

The --detach flag will run this container in the background.
The --publish flag is a feature that can expose networking through the container onto the host.
You are also specifying the --name flag, which names the container.

Access the NGINX server on http://localhost:8080.
Access http://localhost:8081 to see some output from Mongo.

**Remove the containers

docker container ls
docker container stop [container id]
docker system prune  - (removes any stopped containers, unused volumes and networks, and dangling images:)


**Create and build the Docker image.

//create a Dockerfile (without any extension)//

# touch Dockerfile
# vim Dockerfile

FROM ubuntu 
MAINTAINER SUJAY GAONKAR
RUN apt-get update 
CMD ["echo", "Hello this is my first docker image"]

# Esc > wq! (saving the file)

# docker image build -t hello-world .
# docker images (verify your newly build image)
# docker run [image_name or ID]

**Push to a central registry
Log in to the Docker registry account by entering docker login on your terminal:

# docker login 

**Tag the image with your username.

# docker tag hello-world [dockerhub username]/hello-world

**Push the image to central regisrty 

# docker push username/hello-world

**Understand image layers**

Each of the lines in Dockerfile is a layer.Each layer contains only the delta, or changes from the layers before it. To put these layers together into a single running container, Docker uses the union file system to overlay layers transparently into a single view.

Each layer of the image is read-only except for the top layer, which is created for the container. The read/write container layer implements "copy-on-write," which means that files that are stored in lower image layers are pulled up to the read/write container layer only when edits are being made to those files. Those changes are then stored in the container layer.

The "copy-on-write" function is very fast and in almost all cases, does not have a noticeable effect on performance. You can inspect which files have been pulled up to the container level with the docker diff command. For more information, see the command-line reference on the docker diff command.

Because image layers are read-only, they can be shared by images and by running containers. For example, creating a new Python application with its own Dockerfile with similar base layers will share all the layers that it had in common with the first Python application.

**Orchestrate application with docker swarm




