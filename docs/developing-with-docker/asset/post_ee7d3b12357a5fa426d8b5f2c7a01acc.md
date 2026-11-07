
## Using Dockerfiles

Now that we understand what every other Dockerfile instruction does, we can prepare our first Dockerfile. As mentioned earlier, it's just a plain text file. Once you've created a Dockerfile and added all your instructions, you can use it to build an image using the docker build command. The format for this command is:

```
docker build [OPTIONS] <PATH or URL> | -
```

The docker build command takes some arguments and options, you can display them with a short description using the -help switch, as with any other Docker command:

```
docker build -help
```

There are options to tweak the build process by specifying the CPU and memory setup or turning off the cache, for example. You can also tag the new repository after the build, using the -t option, for example:

```
docker build -t jarek/myapp .
```

To tag the image into multiple repositories at once, you can also add multiple -t parameters:

```
docker build -t jarek/myapp:1.0.0 -t jarek/myapp:latest .
```

The default set of parameters related to CPU and memory tweaking should be a good enough starting point, so you can omit them at the beginning.

The docker build command builds an image from a Dockerfile and a context. The build's context is the files at a specified location under the PATH or URL. The PATH is a directory on your local filesystem, while the URL is the location of a Git repository. It works recursively, so a PATH includes any subdirectories and the URL includes the repository and its submodules. If you run the build command from the directory where your actual Dockerfile is, use just a dot as the PATH parameter. The simplest form of docker build, with the current directory as a build context will then be:

```
docker build
```

The build is being run by the Docker daemon. The first thing that will happen is that the entire context will be sent as a single bundle to the daemon. That's why you should be careful when specifying the build context. You can observe the size of the context being pushed to the daemon just after you have started the build:

![](img/0c87321919fa245e22f056c67ec70003_164_0.png)

![](img/0c87321919fa245e22f056c67ec70003_164_1.png)

It's good practice to start with a directory containing only your Dockerfile as context and add only the files that are needed for building the Dockerfile.

Anyway, you can have your Dockerfile anywhere in the system. To build an image using the file outside of the current directory, you can use the -f switch for the docker build command, for example:

```
docker build -f /usr/Jarek/myDockerfiles/Dockerfile .
```

In the preceding example, Docker will use a Dockerfile from the specified path, but the build context will be the current directory, as specified by a dot.

As we have mentioned earlier in this chapter, instructions from Dockerfile, such as COPY, for example, will look for the file in the context of the build if the source path is specified without a leading slash. In real life, the docker build context will contain many other working files and directories, which should never be included in the image, like temporary files, logs, and so on. Nevertheless, the Docker build system would still send those files to the daemon.

To increase the build's performance and make the image as clean as possible, you can exclude files and directories by adding a `.dockerignore` file to the context directory. It's similar to the `.gitignore` file—all paths contained in the `.dockerignore` file will be simply ignored. You can use standard wildcards when specifying paths to ignore. Consider the following example of the `.dockerignore` file:

```
*/tmp*
*/*/tmp*
**/*.log
tmp?
```

In the preceding example, file and directory names start with `tmp` in any immediate subdirectory of the root. Also files and directories starting with `tmpf` from any subdirectory that is two levels below the root and files and directories in the root directory whose names are a one-character extension of `tmp`, such as `tmp1` or `tmp2` will also be ignored. The special mapping in the form of `**/*` can also be used—it matches any number of directories (including zero). For example, `**/*.log` will exclude all the log files found in all the directories. You can exclude some files from being matched to ignored. Consider the following example:

```
*.log
!important.log
```

The preceding example will match all log files, except the `important.log` file. When doing exclusions, be advised that the order of lines in the `.dockerignore` file is important. If you exclude a file and then declare similar matching below that exclusion, it will not be excluded.

Using `.dockerignore` can result in a much more cleaner image. There's another way of customizing the build process. As we have mentioned before, environment variables that are declared with the `ENV` instruction can be used in Dockerfile instructions, such as `ADD`, `COPY`, `ENV`, `EXPOSE`, `LABEL`, `USER`, `WORKDIR`, `VOLUME`, `STOPSIGNAL`, or `ONBUILD`.

The build command, if parsed and processed successfully, results in a new image that you can start using `docker run`, just like any other image, taken, for example, from Docker Hub. The output after executing the `docker build` command will be the ID of the new Docker image. It will also be present if you list your images using the command you already know: `docker images`, as shown in the following screenshot:

![](img/0c87321919fa245e22f056c67ec70003_166_0.png)

### Summary

Docker provides a small but very flexible set of instructions for creating images using Dockerfiles. We have illustrated all the commands, their syntax, and their usage techniques in order to effectively create Docker images. Instructions have a rather simple syntax and are very well documented in the official Docker documentation. Creating your own images will not be very tricky. However, you will need to stick to some best practices and guidelines, recommended by Docker. First of all, you should keep in mind that the containers created by your images should be ephemeral, that is, they should be able to be stopped, destroyed, and recreated with absolute minimum configuration. The images should be clean as far as possible, so starting from an empty build context, adding only what is necessary and using a .dockerignore is a way to go. Also, the YAGNI (You ain't gonna need it) principle, known from clean-code development practices has a very important meaning when it comes to creating Dockerfiles.

Keep it to an absolute minimum, don't install something because it's nice to have it in a container. Always keep in mind the cache mechanism Docker uses and try to minimize the number of layers, by combining similar commands into one instruction. Bare minimum, working as fast as it can and weighing as little as possible should be the goal. Also, if it's possible, consider running only one process per container–it will scale easier and allow for container reuse. If one service depends on another service, they should run in different, but linked containers.

Working with Dockerfiles needs some practice and experimenting, but will pay later–when you will be able to create and adjust your images quickly. In the next chapter, we will focus more on running the images.

## 7

## Running Containers

In previous chapters, we have learned about the Docker architecture, networking, persistent storage, and the structure of Dockerfile. At this point, you should be able to create your own Docker image and start using it. Actually, we did run the containers several times, but without going into details; we are going to do that in this chapter. In this chapter, we are going to focus more on running the containerized software. This will include the following concepts:

- Container running modes
- Overriding the `CMD` and `ENTRYPOINT`
- Monitoring containers
- Exit codes and restart policies
- Runtime constraints on resources
- Docker Swarm mode

You will learn how to set up your containers to behave well when running by specifying the restart policy and runtime constraints. In `Chapter 6, Creating Images`, we already discussed the `CMD` and `ENTRYPOINT` commands of Dockerfile. In this chapter, we will focus on how to override them during the container startup, this time in detail. We will learn how to read containers logs and output them not only to the text file, but to some specialized log tools, such as Splunk or Fluentd, for further analysis or processing. At the end of the chapter, we will learn about the new Docker feature – the Swarm mode, which allows you to manage the cluster of your Docker containers.
