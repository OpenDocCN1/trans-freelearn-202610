Apart from creating a volume when starting a container or using data-only containers, there is a command that allows creating the volume before starting any containers. It's the last option on our list: the possibility to create Docker volumes explicitly, using the docker volume create command.

We will use it now, and this time, we will aslo provide a more friendly name for our volume. To create a volume, execute the volume create command:

```
docker volume create --name data
```

Again, the new data volume will be available when you list volumes using the volume ls command. To mount a volume in a container during its startup, use the same -v option when starting a container:

```
docker run -it -v data:/data ubuntu
```

The data volume will then be available for your container in /data. It's also possible to run another container with the same volume mounted.

![](bbox=[0.12, 0.48, 0.2, 0.6])

Volumes can be shared between containers – just run them with mapping the same volume.

When it comes to creating your own images using Dockerfile, the VOLUME instruction comes in handy. Actually, we will be using it later in the book, when creating images from a Dockerfile.

It creates a mount point with the specified name and marks it as holding externally mounted volumes from native host or other containers. Creating volumes using the VOLUME instruction has one important difference to using option during the container startup: you cannot specify a host directory when using the VOLUME instruction. Dockerfiles are meant to be portable and shared. The host directory volume is 100% host dependent and will break on any other machine, which is a little bit off the Docker idea. Because of this, it is only possible to use portable instructions within a Dockerfile. A benefit of using shared volumes is that they are host-independent. This means that a volume can be made available on any host that a container is started on as long as it has access to the shared storage and has the plugin (driver) installed.

![](bbox=[0.12, 0.83, 0.2, 0.95])

If you need to specify a host directory when creating a volume, you need to specify it at runtime.