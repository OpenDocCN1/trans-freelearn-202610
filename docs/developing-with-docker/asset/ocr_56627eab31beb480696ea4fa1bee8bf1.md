If the RUN command doesn't change between two builds, Docker's cache will not get invalidated. In effect, Docker will reuse the previous results from cache. This is clearly harmful, when, for example, the RUN command is a source code checkout, for example, a Git clone as the first step of a project's build.

![](bbox=[0.13, 0.23, 0.21, 0.33])

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