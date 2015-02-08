# Linking Containers

(incomplete)

Most information here and all docker info have been derived from the official [docs](https://docs.docker.com)

Links allow containers to discover each other and securely transfer information about one container to another container.

When you set up a link, you create a conduit between a *source container* and a *recipient container*. The recipient can then access select data about the source.

What does linking the containers do? A link allows a source container to provide information about itself to a recipient container.

To create a link, use the `--link` flag.

To do this, Docker creates a secure tunnel between the containers that doesn’t need to expose any ports externally on the container.

Here, you’ll see that the recipient, **web**, can access information about the source **db**. 

You’ll notice that you do not have to start the **db** container with either a *-p* or a *-P* flag. That is a the big benefit of linking. You do not need to expose the source container (here, the db), to the network.

Docker exposes connectivity information for the source container to the recipient container in two ways: through environment variables and updating the /etc/hosts file.

Read more [here](https://docs.docker.com/userguide/dockerlinks/#environment-variables) for additional information.

```bash
# create a new container
# this creates a new container called ‘db’ from the ‘training/postgres’ image
sudo docker run -d --name db training/postgres

# create a new ‘web’ container
# and link it with ‘db’ container
# this will link the ‘web’ container
# with the ‘db’ container
# link form: `--link container_name:alias`
sudo docker run -d -P --name web --link db:db training/webapp python app.py

# inspect the linked containers
sudo docker inspect -f “{{ .HostConfig.Links }}” web

# linking containers allow a source container to provide
# info about itself to a recipient container
```
