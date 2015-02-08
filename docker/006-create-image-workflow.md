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
sudo pull ubuntu:14.04

# 
```
