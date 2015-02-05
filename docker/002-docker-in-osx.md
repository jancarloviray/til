# Use Docker in Mac OSX

“Because the Docker Engine uses Linux-specific kernel features, you’ll need to use a lightweight virtual machine (VM) to run it on OSX. You use the OS X Docker client to control the virtualized Docker Engine to build, run and manage Docker containers.” - https://docs.docker.com/installation/mac/

Before continuing, install [Boot2Docker](https://github.com/boot2docker/boot2docker). It will install a virtual machine (using VirtualBox) that is all set up to run the Docker daemon.

[Latest Boot2Docker](https://github.com/boot2docker/osx-installer/releases)

## Quick Start

```bash
# assuming that Boot2Docker is installed...

# initialize boot2docker and download latest ISO
# and generate public/private key pair
boot2docker init

# start VM and docker daemon (server)
boot2docker start

# initialize shell
$(boot2docker shellinit)

# upgrade
boot2docker stop
boot2docker download
boot2docker start

# test run docker
# note that if this image is not found,
# it will force download it and create
# a small container with an executable
# that prints a brief ‘Hello from Docker’
# message
docker run hello-world

# want to download a full-blown ubuntu?
# note that it could be a ~200mb download
# after it downloads, you’ll be inside
# the bash shell. `exit` to exit
docker run -it ubuntu bash
```
