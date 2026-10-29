Apart from spending a few seconds finding the docker IP, this opens nicely there too!

![](bbox=[0.06, 0.11, 0.61, 0.28])

Sweet. Now it gets interesting. Let's now see how we could skip the Docker Hub step and deploy to a remote machine anyway.

# Connecting directly to a remote Docker

When running any of the Docker commands, you can ask the Docker client to connect to either the local daemon or a remote one.

The default daemon is running off a Unix socket, usually located somewhere around /var/run/docker.sock.

If you remember the change of config we did on the Debian machine, or if you have not done so, here is a quick reminder. You will see that we ask the daemon to bind itself to a Unix socket and a TCP socket as well:

```
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:2376
```

The Docker daemon is now able to receive commands from either the local client or a remote Docker client that will use the TCP protocol.

To ask a client to talk to a remote protocol, a DOCKER_HOST property needs to be set up and tasked with assigning the value where the remote Docker daemon is running:

```
export DOCKER_HOST="192.168.1.4:2376"
```