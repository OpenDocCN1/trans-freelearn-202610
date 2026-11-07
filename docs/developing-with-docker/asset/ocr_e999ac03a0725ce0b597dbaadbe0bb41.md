To list all containers you have on your system – either running or stopped – execute the ps command:

```
$ docker ps -a
```

As a result, Docker client will list a table containing container IDs (a unique identifier you can use to refer to the container in other commands), creation date, the command used to start a container, status, exposed ports, and a name (assigned by you or the funny name Docker has picked for you):

![](bbox=[0.1, 0.28, 0.9, 0.43])

As you can see, the output will contain detailed information about the container status and uptime and a command used to start in the container.

To remove a container, you can just use the rm command. If you want to remove all stopped containers at once, you can use the list of containers (given by the ps command) and a filter:

```
$ docker rm -v $(docker ps -q -f
status=exited)
```

# Saving changes to a container

Although an image is always read-only and immutable, we can actually make changes to a running container – the top layer of a container stack is always the read/write (writable) layer. This can be adding or modifying files, such as installing a software package, configuring the operation system, and so on.

![](bbox=[0.2, 0.74, 0.8, 0.85])