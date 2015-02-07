# Common Docker Commands

A summary of commands I have come across to. Could be a cheatsheet in the future after I aggregate all of these.

*Note: if you are using Boot2Docker, then do not type sudo otherwise you will get an error*

### Check Docker Version
#####`sudo docker version`

### Run a Command and Activate a Container
#####`sudo docker run ubuntu /bin/echo ‘Hello world’`

- first, we specified the **Docker** binary and the command we wanted to execute, **run**. Note that the **docker run** combination runs the container.
- Next, we specified an image. If it is not installed, it will download it.
- Next is the command we run inside the container. 
- Note that docker containers are only active so long as the command you specify is active.

### Interactive Container  
#####`sudo docker run -t -i ubuntu /bin/bash`

- **-t** assigns a pseudo-tty or terminal inside our new container
- **-i** flag allows us to make an interactive connection by grabbing the standard in STDIN of the container
- **/bin/bash** also launches a bash shell inside our container

### Daemonize a Container
#####`sudo docker run -d ubuntu /bin/sh -c “while true;do echo hello world; sleep 1; done”`

- **-d** tells Docker to run the container and put it in the background, to “daemonize” it
- note that after you run this, you will not see the output but Docker will rather give you the **Container ID**.
- to see running Containers, run `docker ps`
- to peek inside a specific container, run `sudo docker logs [containerIDorName]`
- to stop running the container, `sudo docker stop [containerIDorName]`. Note that sometimes it might take a few seconds for the stop process to complete.

### List Running Containers
#####`sudo docker ps`

### List Running and Stopped Containers
#####`sudo docker ps -a`

### List Latest Container
#####`sudo docker ps -l`

#### Check Container Log
#####`sudo docker logs [containerID]`

### Stop a Container Running in Background
#####`sudo docker stop [containerID]`

### Expose Container Ports to Host (ie: run a web app)
#####`sudo docker run -d -P training/webapp python app.py`

- **-d** runs the container in background
- **-P** tells Docker to map network ports inside our container to our host

When you run `docker ps` you will see a column like this:
```
PORTS
0.0.0.0:49155->5000/tcp
```

This means that Docker has exposed port 5000 (default python flask port) on port 49155.

**NOTE: if you are using boot2docker, you will need to get the ip of boot2docker. Instead of running localhost:49155, you must get the port by running `boot2docker ip` or `open $(boot2docker ip):49155`**

### Expose Container Port to Host Manually
##### `sudo docker run -d -p 5000:5000 training/webapp python app.py`

This maps container port 5000 to host port 5000.
