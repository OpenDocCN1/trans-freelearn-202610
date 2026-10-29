```
EXPOSE 8080
# what will be executed when you do, docker run ...
ENTRYPOINT [ "java", "-jar", "/app/withdocker.jar"]
```

Assuming you have the Docker daemon running on your local machine by now, you can build the container, named withdocker:

```
docker build . -t withdocker
```

If all goes well, you should see some sexy output:

```
Sending build context to Docker daemon 12.66 MB
Step 1 : FROM java:openjdk-8-jre
---> 76fd51ceaa2e
Step 2 : COPY target/withdocker-0.1.0-SNAPSHOT-standalone.jar /app/withdocker.jar
---> 10df21f2d7b2
Removing intermediate container 6444756f7aaf
Step 3 : EXPOSE 8080
---> Running in 0496cc294447
---> 56446e3f92fe
Removing intermediate container 0496cc294447
Step 4 : ENTRYPOINT java -jar /app/withdocker.jar
---> Running in 030972a7ac62
---> cea5a78eab27
Removing intermediate container 030972a7ac62
Successfully built cea5a78eab27
```

As a reminder, each step in the preceding output corresponds to one line of the Dockerfile.

Now let's start our freshly baked container:

```
docker run withdocker
```

And...Woohoo! Jetty is starting!

```
2016-08-03 16:28:10.912:INFO:oejs.Server:jetty-7.x.y-SNAPSHOT
2016-08-03 16:28:11.257:INFO:oejs.AbstractConnector:Started SelectChannelConnector@0.0.0.0:8080
```

Ah...as you may have noticed already, the page won't open. You still have to tell Docker the ports that it needs to map; while you are at it,