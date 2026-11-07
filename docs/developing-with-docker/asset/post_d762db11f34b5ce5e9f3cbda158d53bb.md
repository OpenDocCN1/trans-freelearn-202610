
As you can see in the preceding Dockerfile, it uses EXPOSE to expose two ports: standard HTTP (80) and SSL (443). These ports will be available for the other containers on this host, and–if you expose them during runtime–also for the external world. You can either run the image using a specified port explicitly:

```
docker run nginx -p 80
```

Alternatively, you can tell Docker that we want all of the exposed ports published at once:

```
docker run nginx -P
```

The good practice is to use common, known port numbers for your application. For example, if your application runs on nginx web server, you should do the EXPOSE with port number 80, so that the users of your application will not be surprised with some exotic port numbers. Of course, if there's a specific need for exposing nonstandard port numbers, you are free to go, it's just a good practice.

## RUN

The RUN instruction is the central executing instruction for Dockerfiles. In essence, the RUN instruction will execute any commands in a new layer on top of the current image and then commit the results. The resulting committed image will be used as a base for the next instruction in the Dockerfile. As you remember from Chapter 3, Understanding Images and Containers, layering is the core concept in Docker. RUN takes a command as its argument and runs it to form the image. The RUN instruction can run any command (or application), which is similar to the CMD instruction we got to know earlier. There is a key important difference–the time of execution. The command supplied through the RUN instruction is executed during the build time, whereas the command specified through the CMD instruction is executed when the container is launched by executing docker run on the newly created image. Unlike CMD, the RUN instruction is actually used to build the image, by creating a new layer on top of the previous one which is committed.

![](img/0c87321919fa245e22f056c67ec70003_153_0.png)

CMD is runtime instruction, where RUN is the build-time instruction.

This also means that CMD and ENTRYPOINT set parameters that can be overridden at runtime, so if you don't change anything, after starting your container, the result will always be the same. RUN, however, will be executed at build time and no matter what you do at runtime, its effects will be seen here.

RUN, similarly to CMD and ENTRYPOINT, can take two forms:

- `RUN <command>`: This is a shell form. The supplied command and its arguments will be executed in a shell, which is `/bin/sh -c` on Linux or `cmd /S /C` on Windows. The default shell for the RUN instruction can be changed using the SHELL command.
- `RUN ["executable", "parameter1", "parameter2"]`: This is an exec form. The same rules apply as in CMD. Using this form, you can execute commands using a base image that does not contain the specified shell executable. RUN in this form will not invoke a command shell. And again, the same as with CMD or ENTRYPOINT, normal shell processing such as variable substitution, for example, will not happen.

![](img/0c87321919fa245e22f056c67ec70003_154_0.png)

To make your Dockerfile more readable and easier to maintain, you can split long or complex RUN statements on multiple lines separated with backslashes.

The RUN commands from the Dockerfile will be executed in the order they appear in it. Each RUN instruction will create a new layer in the image. Layers, as you already know from Chapter 3, Understanding Images and Containers, are being cached and reused by Docker. The cache for RUN instructions isn't invalidated automatically during the next build. For example, the cache for an instruction such as RUN apt-get upgrade -y will be reused during the next build. What makes the cache important? For the most part, the cache is exceptionally helpful and can save you tremendous amount of time while building your images. If the objects on the filesystem that Docker is about to produce are unaffected between builds, reusing a cache of a previous build on the host is a great time-saver. It makes building a new container really, really fast. None of those file structures have to be created and written to disk this time–the reference to them is sufficient to locate and reuse the previously built structures. However, there are times when the caching can be dangerous and provide unexpected results. Cache is used pretty heavily during the build process and this may cause issues when you want the updated output of a RUN command to make it into the new container.

If the RUN command doesn't change between two builds, Docker's cache will not get invalidated. In effect, Docker will reuse the previous results from cache. This is clearly harmful, when, for example, the RUN command is a source code checkout, for example, a Git clone as the first step of a project's build.

![](img/0c87321919fa245e22f056c67ec70003_155_0.png)

Beware that cache needs to be invalidated sometimes, otherwise you will get unexpected results with your image builds.

That's why it's good to know how to selectively invalidate the cache. In the Docker world, this is called cache busting.

Consider the following example. Probably the most common use-case for RUN is an application of apt-get, which is a package manager command for downloading packages on Ubuntu. Let's say we have the following Dockerfile, installing a MongoDB in image:

```
FROM ubuntu
RUN apt-get update
RUN apt-get install -y mongodb-server
```

If we build an image from this Dockerfile, all layers from two RUN instructions will be put into the layers cache. But, after a while you decide you want the node.js package in your image, so now the Dockerfile looks like this:

```
FROM ubuntu
RUN apt-get update
RUN apt-get install -y mongodb-server
RUN apt-get install -y nodejs
```

On the second build, Docker will reuse the layers by taking them from the layer's cache. As a result the apt-get update will not be executed, because the cached version will be used. In effect, your newly created image will potentially have an outdated version of the MongoDb and node-js packages. You should always have the cache concept in mind when creating RUN instructions. In our example, we should always combine RUN apt-get update with apt-get install in the same RUN statement, which will create just a single layer. Consider the following example:

```
RUN apt-get update \
&& apt-get install -y mongodb-server \
&& apt-get install -y nodejs
```

Yet better than this, you can also use a technique called **version pinning** to avoid cache problems. It's nothing else than just providing a specific, concrete version for the package you want to install:

```
RUN apt-get update \
&& apt-get install -y mongodb-server \
&& apt-get install -y nodejs=0.6.12~DFSG1-1UBUNTU1
```

![](img/0c87321919fa245e22f056c67ec70003_156_0.png)

Using `apt-get update` alone in a RUN statement is not recommended because of the caching issues.

The cache for RUN instructions can be invalidated by ADD instructions or using the `--no-cache` flag, for example:

```
docker build --no-cache
```

## USER

The USER instruction is very simple. It sets the user name or UID to use when running the image. It also affects the user for any RUN, CMD, and ENTRYPOINT instructions that follow it in the Dockerfile.

The syntax of the instruction is just USER <user name or UID>, for example:

```
USER tomcat
```

You can use the USER command if an executable can be run without privileges. The Dockerfile can contain the user and group creation instruction like this one:

```
RUN groupadd -r tomcat && useradd -r -g tomcat tomcat
```

![](img/0c87321919fa245e22f056c67ec70003_156_1.png)

Switching USER back and forth frequently will increase the number of layers in the resulting image and will also make a Dockerfile more complex.

## VOLUME

As you remember from Chapter 3, *Understanding Images and Containers*, Docker filesystems are kind of temporary by default. If you start up a Docker image (that is, run the container) you'll end up with a read-write layer on top of the layers stack. You can create, modify, and delete files as you wish, then commit the layer to persist the changes. Also, using Dockerfile will create a read-only image. In Chapter 4, *Networking and Persistent Storage* we have learned how to create volumes—a great way of storing and retrieving data from the Docker container. We can do the same in the Dockerfile, using the VOLUME instruction.

The syntax couldn't be simpler, it's just `VOLUME [ "/volumeName" ]`.

The parameter for VOLUME can be a JSON array, a plain string with one or more arguments, for example:

```
VOLUME ["/var/lib/tomcat8/webapps/"]
VOLUME /var/log/mongodb /var/log/tomcat
```

The VOLUME instruction creates a mount point with the specified name and marks it as holding externally mounted volumes from native host or other containers.

The VOLUME command will mount a directory inside your container and store any files created or edited inside that directory on your host's disk outside the container file structure. Using VOLUME in the Dockerfile lets Docker know that a certain directory contains permanent data. Docker will create a volume for that data and never delete it, even if you remove all the containers that use it. It also bypasses the union filesystem so that the volume is in fact an actual directory that gets mounted, either read/write or read-only, in the right place in all the containers that share it (if they are started with `--volumes-from` option, for example). To understand VOLUME, let's look at the simple Dockerfile:

```
FROM ubuntu
VOLUME /var/myVolume
```

If you now run your container and save some files in `/var/myVolume`, they will be available to other containers for sharing.

Basically, the VOLUME and `-v` options are almost equal. A difference between the VOLUME and `-v` options is that you can use the `-v` option dynamically and mount your host directory on your container when starting it by executing `docker run`. The reason for this is that Dockerfiles are meant to be portable and shared. The host directory volume is something that is 100% host dependent and will break on any other machine, which is a little bit off the Docker idea. Because of this, it is only possible to use portable instructions within a Dockerfile.
