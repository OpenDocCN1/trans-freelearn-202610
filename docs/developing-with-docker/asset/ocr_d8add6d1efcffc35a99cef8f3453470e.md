Parameters can be passed to the builder by calling:

```
docker build --build-arg user=somebodyElse .
```

![](bbox=[0.12, 0.2, 0.2, 0.27])

It is not recommended to use ARG for passing secrets such as GitHub keys, user credentials, passwords, and so on, because it will be visible to any user of the image by using the docker history command.

# ONBUILD

The ONBUILD instruction specifies additional instructions, which will be triggered when some other image is built by using this image as its base image. In other words, the ONBUILD instruction is an instruction the parent Dockerfile gives to the child Dockerfile (downstream build). Any build instruction can be registered as a trigger and those instructions will be triggered immediately after the FROM instruction in the Dockerfile.

The syntax of the ONBUILD instruction is as follows:

```
ONBUILD <INSTRUCTION>
```

Within this, <INSTRUCTION> is another Dockerfile build instruction, which will be triggered later when the child image is going to be built. There are some limitations: the ONBUILD instruction does not allow the chaining of another ONBUILD instruction and it does not allow the FROM and MAINTAINER instructions as ONBUILD triggers.

This is useful if you are building an image that will be used as a base to build other images, for example, an application build environment or a daemon, which may be customized with user-specific configuration. The ONBUILD instruction is very useful for automating the build of your chosen software stack. Consider the following example with Maven and building Java applications. (yes, Maven is also available as a Docker container). Basically, all your project's Dockerfile needs to do is reference the base container containing the ONBUILD instructions:

```
FROM maven:3.3-jdk-8-onbuild
CMD ["java","-jar","/usr/src/app/target/app-1.0-SNAPSHOT-jar-with-dependencies.jar"]
```