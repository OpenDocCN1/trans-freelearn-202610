As you can see in the preceding Dockerfile, it uses EXPOSE to expose two ports: standard HTTP (80) and SSL (443). These ports will be available for the other containers on this host, and–if you expose them during runtime–also for the external world. You can either run the image using a specified port explicitly:

```
docker run nginx -p 80
```

Alternatively, you can tell Docker that we want all of the exposed ports published at once:

```
docker run nginx -P
```

The good practice is to use common, known port numbers for your application. For example, if your application runs on nginx web server, you should do the EXPOSE with port number 80, so that the users of your application will not be surprised with some exotic port numbers. Of course, if there's a specific need for exposing nonstandard port numbers, you are free to go, it's just a good practice.

# RUN

The RUN instruction is the central executing instruction for Dockerfiles. In essence, the RUN instruction will execute any commands in a new layer on top of the current image and then commit the results. The resulting committed image will be used as a base for the next instruction in the Dockerfile. As you remember from Chapter 3, Understanding Images and Containers, layering is the core concept in Docker. RUN takes a command as its argument and runs it to form the image. The RUN instruction can run any command (or application), which is similar to the CMD instruction we got to know earlier. There is a key important difference–the time of execution. The command supplied through the RUN instruction is executed during the build time, whereas the command specified through the CMD instruction is executed when the container is launched by executing docker run on the newly created image. Unlike CMD, the RUN instruction is actually used to build the image, by creating a new layer on top of the previous one which is committed.

![](bbox=[0.13, 0.72, 0.21, 0.78])

CMD is runtime instruction, where RUN is the build-time instruction.