# On Creating an Image

## Create a Dockerfile

```Bash
# Base Image
FROM ubuntu:14.04

# Install
RUN \
    apt-get update && \
    apt-get install -y 
```

```
# build the image
sudo docker build -t [userName/imageName:tag] .
```

## Manual

```
# check locally saved images
sudo docker images

# search for available images
sudo docker search ubuntu

# pull required base os to cache
# `$ pull [image:tag]`
sudo pull ubuntu:14.04

# create and activate a container
# (-d) run as daemon
# (-it) interactive and attach a terminal
# (--name) custom name instead of a random one
# `$ docker run -d -it --name [container] [baseImage] [command]`
sudo docker run -d -it --name development ubuntu:14.04 /bin/bash

# check if running
sudo docker ps

# go back into container
# and straight into the shell
# once you are inside, you’re in a “VM”!
# now do your changes, or 
# run commands, apt-get, mkdir, touch, etc
sudo docker exec -it development /bin/bash

# (inside container) --------------------------
apt-get update
apt-get -y install build-essential libssl-dev software-properties-common
apt-get -y install curl wget vim man htop unzip git zsh tmux

# change default shell to zsh
chsh -s /usr/bin/zsh

# install nvm
curl https://raw.githubusercontent.com/creationix/nvm/v0.23.3/install.sh | bash
source ~/.profile
nvm install stable && npm alias default stable

# install oh-my-zsh
curl -L http://install.ohmyz.sh | sh

# configure locale (for bad char encoding fixes)
sudo locale-gen en_US en_US.UTF-8
dpkg-reconfigure locales
apt-get install language-pack-en-base
echo ‘export LC_ALL=”en_US.UTF-8”’ >> ~/.zshrc

# (/inside container) -------------------------

# (back to host)
# check size of containers
sudo docker ps -s

# once done, exit from container 
# and go back to host to check diff
sudo docker diff development

# commit container into a brand new image!
# `$ docker commit -m [comment] -a [author] [container] [image:tag]`
# TIP: rm -rf /var/lib/apt/lists/* to remove lists
sudo docker commit \
-m ‘Added node, vim,...’ \
-a ‘Jan Carlo Viray’ \
development \
jancarloviray/development:0.1

# check if images exist
docker images

# push to the hub!
docker push jancarloviray/development
```
