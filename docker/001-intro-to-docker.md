# Docker Intro

Docker is a growing technology for and spreading like wildfire. Time to delve in the basics and apply it to all my projects!

Docker is here to offer an efficient, speedy way to port applications across systems and machines. It is light and lean, allowing you to quickly contain applications and run them within their own secure environments (via Linux Containers: LXC).

“Docker is an open platform for developing, shipping and running applications. Docker is designed to deliver your applications faster. With Docker you can separate your applications from your infrastructure AND treat your infrastructure like a managed application. Docker helps you ship code faster, test faster, deploy faster, and shorten the cycle between writing code and running code.” - docker.com

## Main Docker Parts

- **docker daemon** is used to manage docker (LXC) containers on the host it runs. The user does not directly interact with the daemon, but instead through the Docker client.
- **docker CLI** is used to command and communicate with the docker daemon in the form of the `docker` binary.
- **docker image index** is a repository for docker images

## Main Docker Elements

### Docker Containers

The entire procedure of porting applications using docker relies solely on the shipment of containers. Docker containers are basically directories which can be packaged (tar-archived) like any other, then shared and run across various different machines and platforms.

The containment here is obtained via Linux Containers (LXC).

Docker containers allow: application portability, isolating processes, prevention from tempering with outside, managing resource consumption.

Think of containers as a process in a box. The box contains everything the process might need. It has a filesystem, has system libraries, shell and etc.

By default, a container that you have pulled from the Registry will not be running. You can start a container using `docker run [imagename] [command to run plus args]`. After the container has executed the command, it will stop the container.

Whatever the command does will not be forgotten within the image. If you install something through apt-get, it will not be forgotten. However, it will not yet be persistent.

To save a persistent change, you can ‘commit’ it by running `docker commit [id] [new/name]`. You typically only need to put in the first two or three characters of the ID of the container. To find the ID, run `docker ps -l`. 

The commit will be a new repo, and will return a new image ID.

To check running containers, use `docker ps`.

To inspect a container, run `docker inspect [containerId]`.

### Docker Images

Docker images constitute the base of docker containers from which everything starts to form. They are very similar to OS disk images which ares used to run apps on servers or desktop computers.

As more layers are added on top of the base, new images can be formed by **committing** these changes. The **union file system** brings all the layers together as a single entity when you work with a container.

### Docker Files

Docker files are scripts containing a successive series of instructions, directions and commands which are to be executed to form a new docker image.

## Quick Start

```bash
# check docker commands
docker

# check docker version
docker version

# search for a docker image in Docker Hub Registry
docker search [name]

# downloading container images
# note that a repo might download other repo,
# making the original repo consist of many layers
docker pull [full/name/of/repository]

# start the container and make it run a command
# note that it will stop the container once it
# finishes executing the command
docker run [full/name/of/image] echo ‘hello world’

# install a program and make it persist
# note that whatever you do to the container will
# not be forgotten, but you need to run `commit` to 
# persist the changes.
# here, we use -y argument to run the command 
# in non-interactive mode
docker run [full/name/of/image] apt-get install ping -y

# get container ID
docker ps -l

# persist changes to the container
# you only need minimum of 3 or 4 characters here.
# note that this will return the new image ID.
docker commit [containerID] [new/repo/name]

# run command on new container ID
docker run [newContainerID] [run some command]

# inspect container
docker inspect [containerID]

# check local images
docker images

# push to registry (must be logged in)
docker push [full/repo/name]
```
