# Removing a volume

The same as with creating volumes, there are two ways of removing a volume from Docker. One way is to remove a volume by referencing a container's name and executing the rm -v command:

```
docker rm -v <containerName>
```

![](bbox=[0.103, 0.242, 0.894, 0.447])

Docker will not warn you when removing a container without providing the -v option to delete its volumes. As a result, you will have dangling volumes – volumes that are no longer referenced by a container.

You can also remove the volume using the volume rm command:

```
docker volume rm <volumeName>
```

If the volume happens to being used by the container, Docker Engine will not allow deleting it and will give you a warning message:

![](bbox=[0.103, 0.67, 0.894, 0.887])