# Common Docker Commands

A summary of commands I have come across to. Could be a cheatsheet in the future after I aggregate all of these.

*Note: if you are using Boot2Docker, then do not type sudo otherwise you will get an error*

## Tips

- You can replace [containerID] with [containerName]
- You only typically need 3 characters from [containerID]
- Append --help if you want more information about the command
- Note that Containers are not the same as Images. Containers are the instances.

## Commands

### General
---

#### Check Docker Version
`sudo docker version`

### Images
---

#### List Locally Saved Images
`sudo docker images`

#### Pull and Cache Images from Remote
`sudo pull [imageName:tag]`

- note that if you do `docker run ubuntu ...` it will run this command if you do not have ubuntu stored locally

#### Search For Available Images
`sudo docker search [imageName]`

### Container Information
---

#### List Containers
```
# list only running containers
sudo docker ps

# list all container
sudo docker ps -a

# show recently created container
sudo docker ps -l

# display file sizes (of all containers)
sudo docker ps -a -s
```

#### View Container Logs
```
# View Container Log
sudo docker logs [containerID]

# View and Tail Container Log
sudo docker logs -f [containerID]

# Also show Timestamps
sudo docker logs -f -t [containerID]
```

#### Inspect a Container
`sudo docker inspect [containerID]`

### Container Actions
---

#### Run a Command and Activate a Container 
`sudo docker run ubuntu /bin/echo ‘Hello world’`

- first, we specified the **Docker** binary and the command we wanted to execute, **run**. Note that the **docker run** combination runs the container.
- Next, we specified an image. If it is not installed, it will download it.
- Next is the command we run inside the container. 
- Note that docker containers are only active so long as the command you specify is active.

#### Interactive Container  
`sudo docker run -t -i ubuntu /bin/bash`

- **-t** assigns a pseudo-tty or terminal inside our new container
- **-i** flag allows us to make an interactive connection by grabbing the standard in STDIN of the container
- **/bin/bash** also launches a bash shell inside our container

#### Run a Container in Background
`sudo docker run -d ubuntu /bin/sh -c “while true;do echo hello world; sleep 1; done”`

- **-d** tells Docker to run the container and put it in the background, to “daemonize” it
- note that after you run this, you will not see the output but Docker will rather give you the **Container ID**.
- to see running Containers, run `docker ps`
- to peek inside a specific container, run `sudo docker logs [containerIDorName]`
- to stop running the container, `sudo docker stop [containerIDorName]`. Note that there is a default timer of 10 seconds before the command will actually execute - a grace period just in case you want to change your mind. To override this, use `sudo docker stop -t 0 [containerID]`

#### Run and Name a Container
`sudo docker run --name my_app_name ubuntu /bin/echo ‘Hello World’`

#### Stop a Container Running in Background
`sudo docker stop [containerID]`

#### Restarting a Container
`sudo docker restart [containerID]`

#### Removing a Container
`sudo docker rm [containerID]`

### Container Networking
---

#### Expose Container Ports to Host (ie: run a web app)
`sudo docker run -d -P training/webapp python app.py`

- **-d** runs the container in background
- **-P** tells Docker to map network ports inside our container to our host

When you run `docker ps` you will see a column like this:
```
PORTS
0.0.0.0:49155->5000/tcp
```

This means that Docker has exposed port 5000 (default python flask port) on port 49155.

**NOTE: if you are using boot2docker, you will need to get the ip of boot2docker. Instead of running localhost:49155, you must get the port by running `boot2docker ip` or `open $(boot2docker ip):49155`**

#### Expose Container Port to Host Manually
`sudo docker run -d -p 5000:5000 training/webapp python app.py`

This maps container port 5000 to host port 5000.
