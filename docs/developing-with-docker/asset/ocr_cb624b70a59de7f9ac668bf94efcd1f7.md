# Exposing and mapping ports

It's a common scenario that you will want your containerized application to accept incoming connections, either from other containers or from outside of Docker. It can be the application server listening on port 80 or the database accepting incoming requests. You can expose a port in two ways, either in the Dockerfile with the `EXPOSE` instruction (we will do it in the chapter about creating images later) or in the run command using the `--expose` option. These are equivalent commands, though `--expose` will accept a range of ports as an argument (for example, `--expose=1000-2000`).

You can also explicitly bind a port (or group of ports) from the container to the host using the `-p` flag. This configuration depends on the host, so there is no equivalent instruction allowed us in the Dockerfile. Dockerfiles need to be host independent and portable.

![](bbox=[0.12, 0.48, 0.2, 0.58])

You can bind a port using `-p` at runtime only.

So, what's the difference between `--expose` and `-p` ? Well, `--expose` will expose a port at runtime, but will not create any mapping to the host.

![](bbox=[0.12, 0.7, 0.2, 0.8])

If you `EXPOSE` a port, the service in the container is not accessible from outside Docker, but from inside other Docker containers. So this is good for intercontainer communication.

The `EXPOSE` instruction exposes ports for use within links, which we are going to explain in a while.

The `-p` option, on the other hand, is like publish – it will create a port mapping rule, mapping a port on the container with the port on the host system. If no port on the host is specified, Docker will automatically allocate one. Note that if you execute `-p`, but do not execute `EXPOSE`, Docker does an implicit `EXPOSE`. This is because if a port is open to the public, it is automatically also open to other Docker containers.