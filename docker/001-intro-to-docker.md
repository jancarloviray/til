# Docker Intro

Docker is a growing technology for and spreading like wildfire. Time to delve in the basics and apply it to all my projects!

## Docker Engine Parts

The Docker Engine consists of two parts: 

- *daemon*, a server process that manages all containers
- *client*, which acts as a remote control for the daemon

## Docker Container

Think of containers as a process in a box. The box contains everything the process might need. It has a filesystem, has system libraries, shell and etc.

By default, a container that you have pulled from the Registry will not be running. You can start a container using `docker run [imagename] [command to run plus args]`. After the container has executed the command, it will stop the container.

Whatever the command does will not be forgotten within the image. If you install something through apt-get, it will not be forgotten. However, it will not yet be persistent.

To save a persistent change, ie: creating a snapshot of the image, you can ‘commit’ it by running `docker commit [id] [new/name]`. You typically only need to put in the first two or three characters of the ID of the container. To find the ID, run `docker ps -l`. 

The commit will be a new repo, and will return a new image ID.

To check running containers, use `docker ps`.

To inspect a container, run `docker inspect [containerId]`.

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
