# Running the container in detached mode

You can also run the container in detached mode using the following command:

```
```bash
docker run -p 8080:8080 cargo-tracker -d
```
```

Note that this command contains an extra parameter, –d. This parameter tells Docker Engine that we want to start the container in detached mode. When you start the container this way, you won’t see the logging of the container. Instead, the command returns the container ID of the container you just started.

To see which containers you have running, you can use the following command:

```
```bash
docker ps
```
```

This will print something like this to the console:

```
```text
CONTAINER        ID     IMAGE          COMMAND                 CREATED       STATUS       PORTS                    NAMES
c94fa6f7accf     cargo-tracker   "/tini -- /bin/sh -c…"   2 minutes ago   Up 2 minutes   4848/tcp, 8181/tcp, 9009/tcp,
0.0.0.0:8080->8080/tcp   inspiring_morse
```
```

This overview shows you the following:

+   - The container ID
+   - The image name
+   - The command
+   - The time the container was created
+   - The status of the container
+   - The port mappings
+   - The name of the container

When you run the container in detached mode, you can close the terminal, and the container will continue to run in the background.

# Stopping a running container

If you want to stop the container, you can use the kill command and pass it as part of the container ID. The following example command kills a container with a container ID that starts with c:

```
```bash
docker kill c9
```
```