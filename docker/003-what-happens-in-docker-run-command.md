# What happens in a `docker run` command?

More specifically, what happens when you run this code: `sudo docker run -i -t ubuntu /bin/bash`?

Pulling from [docs](docs.docker.com), this is what docker does:

- *Pulls the ubuntu image*: docker checks for the presence of the ubuntu image and, if it doesn’t exist locally, then Docker downloads it from [Docker Hub](hub.docker.com). If image exists, then Docker uses it.
- *Creates a new container*: once docker has the image, it uses it to create a container.
- *Allocates a file system and mounts a read-write layer*: The container is created in the file system and a read-write layer is added to the image.
- *Allocates a network/bridge interface*: creates a network interface that allows the Docker container to talk to the local host.
- *Sets up an IP address*: Finds and attaches an available IP address from a pool
- *Executes a process that you specify*: runs your application, and
- *Captures and provides application output*: connects and logs standard input, output and errors for you to see how your application is running.
