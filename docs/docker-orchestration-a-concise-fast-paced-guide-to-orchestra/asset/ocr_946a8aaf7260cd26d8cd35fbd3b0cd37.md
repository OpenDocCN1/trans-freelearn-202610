+   - **Do not forget the network**: When running a cluster locally, it can be easy to overlook, or take for granted, the network infrastructure. Where feasible, add the networking stack to your monitoring. Some organizations split the network and server operations teams which may make adding the network stack to the Docker monitors politically problematic. In that case, make friends with the network team.
+   - **Create useful dashboards**: Being able to see the logs and metrics from related components in one place can simplify troubleshooting. The trick is to know what is worth looking at and what is not. Too much information can be just as much a problem as too little.

# # Watching services in real time with Sysdig

Sysdig is a real-time command-line monitoring tool. It can monitor most host components and has support for reporting container statistics. It also has support for viewing metrics based on Kubernetes pods, replication controllers, services, and more.

When possible, Sysdig should be installed and run directly on the host machine. This will provide it the most visibility into the system. Packages are available for Debian and Red Hat-based systems. Installation instructions may be found at http://www.sysdig.org/install/.

It is also possible to run Sysdig in a container. This is especially useful if the Docker hosts are running a Linux distribution that is not directly supported or systems such as CoreOS which run everything in containers. The command to start the container is as follows:

```
$ docker run -it --rm --name=sysdig --privileged=true \
--volume=/var/run/docker.sock:/host/var/run/docker.sock \
--volume=/dev:/host/dev \
--volume=/proc:/host/proc:ro \
--volume=/boot:/host/boot:ro \
--volume=/lib/modules:/host/lib/modules:ro \
--volume=/usr:/host/usr:ro \
sysdig/sysdig
```

There are a couple of things to note:

+   - First of all, running `sysdig` in a container requires the use of the `--privileged` flag. It needs the access in order to load the `sysdig` kernel module and to be able to access all of the required information.
+   - Second, the command maps through a number of important file systems such as `/proc` for visibility reasons.