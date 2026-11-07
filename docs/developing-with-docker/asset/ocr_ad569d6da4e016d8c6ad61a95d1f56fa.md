Yet better than this, you can also use a technique called **version pinning** to avoid cache problems. It's nothing else than just providing a specific, concrete version for the package you want to install:

```
RUN apt-get update \
&& apt-get install -y mongodb-server \
&& APT-GET INSTALL -Y NODEJS= 0.6.12~DFSG1-1UBUNTU1
```

![](bbox=[0.12, 0.26, 0.2, 0.32])

Using `apt-get update` alone in a RUN statement is not recommended because of the caching issues.

The cache for RUN instructions can be invalidated by ADD instructions or using the `--no-cache` flag, for example:

```
docker build --no-cache
```

# USER

The USER instruction is very simple. It sets the user name or UID to use when running the image. It also affects the user for any RUN, CMD, and ENTRYPOINT instructions that follow it in the Dockerfile.

The syntax of the instruction is just USER <user name or UID>, for example:

```
USER tomcat
```

You can use the USER command if an executable can be run without privileges. The Dockerfile can contain the user and group creation instruction like this one:

```
RUN groupadd -r tomcat && useradd -r -g tomcat tomcat
```

![](bbox=[0.12, 0.78, 0.2, 0.84])

Switching USER back and forth frequently will increase the number of layers in the resulting image and will also make a Dockerfile more complex.