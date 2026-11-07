
As you can see, having the Docker Swarm is not that tricky as it may seem. The beauty of the Docker cluster implementations is its simplicity. With the swarm mode you create a swarm with the `init` command, and add workers to the cluster with the `join` command. That's it. The commands to create and join a swarm literally take a second or two to complete.

## Summary

You may have an impression, that there is a huge flexibility when it comes to running Docker containers. And you are right, definitely there is much we can do. We can override the images `RUN`, `CMD`, `ENTRYPOINT` instructions to change the container run behavior, sometimes even totally change the container's purpose if needed. Docker provides a lot of powerful commands to inspect the container runtime state, viewing logs, events and statistics. They surely make devops' daily job a lot easier. Also, setting the container restart policies and runtime constraints on CPU or memory can result in a run-and-forget way of working with Docker containers. If you need your applications to scale easily, there is nothing simpler than bringing a Docker Swarm to life. Setting up Docker Swarm is easy, straightforward and flexible. Of course, detailed setup of a cluster may take some time, but running a basic cluster of Docker containers in seconds is possible.

Now that we know how to create images, how to run and test them during runtime – it's time for another step – to show the world our work. In the next `Chapter 8`, *Publishing Images*, we are going to publish our images to the Docker Hub and do some more interesting tricks, like creating the automated builds for our images.

## 8

## Publishing Images

So far you have learned how to use the command line to run Docker on your local machine, how to pull down images to build containers from existing images, and how to create your own images. In this chapter, we are going to learn how to publish our images in the Docker Hub registry. We will also cover the process of doing an automatic build of the image using the Webhooks. We have already discussed the process of pulling an image from the registry in Chapter 5, Finding Images. Now, however, instead of pulling images, we are going to publish our work. As you might remember, to do this, you will need to have a Docker Hub account. You can refer to Chapter 5, Finding Images, for the instructions on how to create an account, if you haven't done so already. The Docker Hub service is free for public Docker images. Once you have created your account, you can push the image that you created previously, to make it available for others to use. In this chapter, we are going to cover the following topics:

- Managing image tags and pushing to the remote registry
- Integrating GitHub and Bitbucket
- Webhooks and automated builds

Let's begin with publishing our images to the repository.

### Publishing images

When dealing with images on the remote registry, the concept of tag is very important. We were able to find the images and identify those using tags. This is also crucial if you are going to publish your own image; you will need to tag it first. Let's do that now.

### Tagging

We use the `docker tag` command to tag an image. You can identify an image you want to tag by either using its ID or its name. Also, you can pick the image to tag using other tags. The syntax of the `docker tag` command is as follows:

```
docker tag IMAGE[:TAG] IMAGE[:TAG]
```

For example, you will need to execute the following command to tag a local image with name `myHelloWorld` and tag `test` into the `myRepo` repository with `version1.0.test`:

```
docker tag myHelloWorld:test myRepo/myHelloWorld:version1.0.test
```

If you run the `docker images` command now, you will see your newly tagged image.

Your newly tagged image is now ready to be pushed to the remote registry. The concept of tags is used heavily by the Docker Hub's automated builds. A tag lets you link a `version` tag to either a branch or a tag in the `git` history. A branch in this case will refer either to a different Git branch or a different sub-directory. We will get back to it in a while when we discuss the automated build feature.

### Untagging the image

Sometimes it may be useful to remove a tag from the image. There is no `docker untag` command, but if you are careful, you can use the `docker rmi` command to untag the image. Normally, we use the `docker rmi` command to remove the image; however, if your image is tagged with more than one tag, then the `docker rmi` command will remove the tag but not the image.

![](img/0c87321919fa245e22f056c67ec70003_203_0.png)

Be cautious, because if the image has no more tags, the `docker rmi` command will remove the image.

For example, execute the following command if you need to remove the `version1.0.test` tag from your image:

```
docker rmi <image ID or name>:version1.0.test
```

### Pushing the image

Once your image is properly tagged, you can log in to the Docker Hub using the docker `login` command, the same way that we did in Chapter 5, *Finding Images*. To log in into the remote registry, execute the docker `login` command:

```
docker login --username=yourUsername --email=yourEmail@address.com
```

After successful authentication, you just simply push the image using the docker `push` command:

```
docker push yourUsername/myRepo
```

Of course, pushing the image can take some time, depending on the image size and the speed of your internet connection. When finished, the image is available for pulling. If you need to check if the image is available and ready on the remote registry, you can just pull it.

![](img/0c87321919fa245e22f056c67ec70003_204_0.png)

If you want to pull your own image from the Docker Hub, first delete the original image from your local machine. Docker will compare the one you have locally and the one you have on the remote registry and will not pull as the local and the remote images are identical.

If publishing to a public repository in the Docker hub, logging in and executing the docker `push` command will make the new image available to the world. However, you are, of course, not limited to the public registry, you can also push the image to your own private registry. In case you want to use a private registry, you need to set the URL to your private registry as the username. First, you will need to tag it properly and then push it. If your registry is listening on localhost port 5000, you can execute the following commands:

```
docker tag yourUser/myRegistry localhost:5000/myRegistry
docker push localhost:5000/myRegistry
```

You can create your own private registry by making use of the Docker Distribution toolset, available freely on GitHub: https://github.com/docker/distribution. The main utility you can find in this GitHub repository is the Docker registry 2.0 implementation for storing and distributing Docker images. It replaces the old docker-registry project with a new API design, focused on security and performance. This new registry implementation provides the following benefits over the old one: faster push and pull, simplified deployment, pluggable storage backend, and webhooks notifications. It supports multiple storage backends, you can choose, for example, the local file system or a cloud similar to the Amazon S3.

Tagging and pushing images to the remote registry, either public or private, is rather straightforward and not a complicated process. But there's more in what Docker Hub offers and automated builds. Let's explain them in the succeeding sections.

### Webhooks and automated builds

As you may recall from Chapter 5, Finding Images, one of the coolest Docker Hub features are the automated builds. An automated build is triggered by webhooks when changes are made to a source code of the image (that is, to Dockerfile). Webhooks let you execute different actions after pushing an image to a repository. Automated Builds repositories automate the building and updating of images from the most popular git repositories, namely GitHub or Bitbucket, directly on Docker Hub. Automated builds repositories can be created either for private or public repositories. It works by adding a commit hook to your selected GitHub or Bitbucket repository; thus, triggering a build and update when you push a commit. Automated Builds repositories have several advantages, images built this way are built exactly as specified, the Dockerfile is available to anyone with access to your Docker Hub repository, and your Docker repository is automatically always up-to-date with source code changes.

### Setting up the automated build

In Chapter 6, Creating Images, we discussed that Docker builds images using the so called build context. The build context is a Dockerfile and any files in a specific location that are referenced by the Dockerfile. For an Automated Build, the build context will be a source code repository containing a Dockerfile. This means that the source code repository must contain all the files that you would usually need to build the image locally, with a mandatory Dockerfile in the first place.

![](img/0c87321919fa245e22f056c67ec70003_205_0.png)

For automated builds, the source code repository is the build context.
