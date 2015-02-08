# Managing Data in Containers

Docker Volumes are for persistence.

Note that containers persist until you remove them with `docker rm [container]`. Until then, the container (an ‘instance’ of an ‘image’ per se) will continue to exist, and can be started, stopped, restarted and etc.

Even if a container is in a “stopped” state, its data is still persisted.

Note that `docker run [container] [command]` actually creates a container, then activates it.

There are two primary ways you can manage data in Docker:

- Data Volumes
- Data Volume Containers or “data only containers”

## Data Volumes

A data volume is a specially-designated directory within one or more containers that bypass the Union File System to provide several useful features for persistent or shared data:

- volumes are initialized when a container is created
- data volumes can be shared and reused between containers
- changes to a data volume are made directly
- changes to a data volume will not be included when you update an image
- volumes persist until no containers use them

### Adding a data volume

You can add data volume to a container using the `-v` flag with `docker create` and `docker run` command. You can use `-v` multiple times to mount multiple data volumes. Let’s mount a single volume now in our web application container.

`sudo docker run -d -P --name web -v /webapp training/webapp python app.py`

This will create a new volume inside a container at /webapp. Note that you can also use the `VOLUME` instruction in a `Dockerfile` to add one or more new volumes to any container created from that image.

## Data Volume Containers or “data only containers”

Data only container is run on a barebone image and does nothing except exposing a data volume. Then you can run any other container to have access to the data container volumes.

If you have some persistent data that you want to share between containers, or want to use from non-persistent containers, it is best to create a **“named data volume container”** and then to mount the data from it.

`docker run --volumes-from [dataContainer] [someOtherContainer] [commandToExecute]`
