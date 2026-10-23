The problems that arise when clocks for various servers are out of sync can be quite mysterious and hard to isolate. That’s why it’s one of the first questions support is likely to ask if you have a really odd problem. Also, when looking at logfiles and trying to correlate entries from different servers, it can be quite confusing if the system clocks are different by even a few seconds. For fully detailed logging, it is not unusual to see literally thousands of lines of logging within a single clock second.

# File storage area

A *storage area* is CE jargon for the place where actual content bits for documents will be stored (as opposed to the properties; those are always stored in the database). To make our CM installation somewhat realistic, we will be using a file storage area. A file storage area resides on a operating system filesystem. In a normal production environment, you would likely have a dedicated file server where the disks actually reside, and multiple CE servers would see folders shared via a supported file sharing protocol (for example, NFS). All CE servers in a domain must be able to reach all storage areas. We don’t really need that complexity for an all-in-one image. The disks will also be local. However, you can still configure it that way if you want things to more closely match a production environment.

We create a single file storage area in the local filesystem and arrange it so that only ecmuser has access (because, as described later, we will be running the WAS profile that holds our CE as ecmuser). The following commands, to be run as the root user, can be used to create and configure the top-level directories that will be needed:

```
# mkdir /p8filestores
# mkdir /p8filestores/fs1
# chown -R ecmuser /p8filestores
# chmod -R 770 /p8filestores
```

The names are arbitrary, but we will need to remember them later. If you were using file sharing, you would instead be creating a mount point for the parent of the filestore directories (/p8filestores in this example).