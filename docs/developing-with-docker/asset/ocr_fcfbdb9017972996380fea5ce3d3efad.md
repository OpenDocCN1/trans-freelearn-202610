![](bbox=[0.13, 0.11, 0.21, 0.17])

The quickest way of determining the current port mapping is just the ps command (as you remember from the previous chapters, it's being used to see the list of running containers).

The -p option, on the other hand, gives us more control. In this case, Docker will not automatically pick any port – it's up to you what ports on the host should be mapped to the container's ports. The syntax of the -p option is quite straightforward: you just enter the container exposed port number, a colon, and then a port you would like to be mapped on the host machine:

```
docker run -p <containerPort>:<hostPort> <image ID or name>
```

If you need to, you can also expose a whole range of ports to other containers using either the EXPOSE instruction in a Dockerfile (EXPOSE 7000-8000, for example) or the run command:

```
docker run --expose=7000-8000 <container ID or name>
```

Of course you can then publish a range of ports to the host machine via Docker's run (https://docs.docker.com/engine/reference/commandline/cli/) command:

```
docker run -p 7000-8000:7000-8000 <container ID or name>
```

Ok, let's stop our nginx using the stop command and start another container, this time mapping the container port 80 (which is exposed in the Dockerfile) to our host machine port number 8080:

```
docker run --name nginx -d -p 8080:80 nginx
```

![](bbox=[0.1, 0.62, 0.9, 0.86])