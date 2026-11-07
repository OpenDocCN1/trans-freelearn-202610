
| Authorize a token for user's repository | PUT /v1/repositories/(namespace)/(repo_name)/auth |
| Verify a user login | GET /v1/users |
| Create a new library repository | PUT /v1/repositories/(repo_name)/ |
| Create a new user repository | PUT /v1/repositories/(namespace)/(repo_name)/ |
| Update user repository images | PUT /v1/repositories/(namespace)/(repo_name)/images |
| Update library repository images | PUT /v1/repositories/(repo_name)/images |
| Delete the existing library repository | DELETE /v1/repositories/(repo_name)/ |
| Delete the user repository | DELETE /v1/repositories/(namespace)/(repo_name)/ |
| List images in the library repository | GET /v1/repositories/(repo_name)/images |
| List images in user's repository | GET /v1/repositories/(namespace)/(repo_name)/images |
| Retrieve an image | GET /v1/images/(image_id)/json |
| Obtain all tags or a specific tag of a repository | GET /v1/repositories/(namespace)/(repository)/tags |
| Delete a tag | DELETE /v1/repositories/(namespace)/(repository)/tags/(tag*) |
| Extract image layer | GET /v1/images/(image_id)/layer |
| Insert image layer | PUT /v1/images/(image_id)/layer |
| Retrieve roots of an image | GET /v1/images/(image_id)/ancestry |

All of the API commands are pretty well documented on the Docker documentation website, available at https://docs.docker.com. There you can find example JSON payloads you need to provide for your API calls, altogether with example responses and HTTP return status codes. The exposed REST API is a great way if you want to develop your own applications or automation scripts dealing with the Registry. This is another proof of extensibility and flexibility Docker provides for administrators or developers.

After reading this chapter, you should now be familiar with the ways you can look for images, be it using the web interface, command-line tool, or remote API. You should be able to easily find an image that will become a base for your own images, which we are going to create in the next chapter, actually.

## Summary

After reading this chapter, you should have the understanding about the concept of Docker's remote storages-Docker Registries. We also know how to create and manage user's account on the most popular Docker registry, the Docker Hub. We are able to find an image that can be useful and appropriate as a base for our own image. In fact, in Chapter 6, Creating Images, we will use all of it in practice—we will create an image based on the image found in the Docker Hub.

## 6

## Creating Images

After reading the previous chapters, you should be familiar on how to find images suitable as a base for your own software. You can pull them from the remote repository, run and make changes to them during runtime and then export as new images. We saw building our own image earlier via running a container, installing our software in it, and doing a commit to create the image in Chapter 3, Understanding Images and Containers. This is not very practical and sometimes very cumbersome. Luckily, there's a solution for that. Docker has a scripting engine that we can use to create a new image with a predefined list of instructions and properties. We call these scripts Dockerfiles. This time, we are going to create an image based on a Dockerfile. This chapter will cover the following topics:

- Introduction to Dockerfiles
- Creating and editing Dockerfiles
- Dockerfile instructions
- Building images

Let's understand what a Dockerfile is first. It's nothing more than a plain text file containing instructions on how to build an image. Docker will process this file and build images step-by-step (or rather, layer-by-layer), automatically, taking a source (base) image as a foundation. We can, for example, use the base Ubuntu image, install the application server such as Wildfly, configure your own application or service, and publish the resulting image as a whole–a complete application that others can use. Using Dockerfile has a lot of benefits–you edit and work on a text file, rather than making all the changes directly and saving the image back to disk. You treat the Dockerfile as a part of the source code of your application–you should put it into the version control system like GIT to be able to track changes and share it with members of your team. In fact, writing a Dockerfile is a more consistent and repeatable way to build your own images. Also, using Dockerfile is the recommended way of building images on the fly, for example, in the continuous deployment setup.

This chapter will cover the following topics:

- Creating and editing Dockerfiles
- Dockerfile instructions
- Building images

Let's begin by creating a Dockerfile. We are going to create images in a more programmatic way using the Dockerfile. To edit Dockerfile, you can use any text editor of your choice, but it would be a lot more convenient to use an IDE with syntax highlighting support for Dockerfiles. For example, IntelliJ IDEA has a Docker integration plugin I can highly recommend. By the way, we will be using this plugin a lot in the next chapter–there is a new IDEA Run/Debug Configuration for Docker deployments, which will allow you to specify the cloud deployment target, an appropriate Dockerfile, and give your container a name. This plug-in lets you download and build images, create and start containers, and carry out other Docker-related tasks. Apart from that, you will get editor completion and syntax highlighting when editing Dockerfiles, as you can see in the following screenshot:

![](img/0c87321919fa245e22f056c67ec70003_136_0.png)

The image creation flow in Docker is pretty straightforward and consists basically of two steps:

- First, you prepare `Dockerfile`, which is just a text file that has a series of instructions on how to build the image. It supports a not very large set of commands that you can use in your Dockerfile, to instruct Docker how to create an image.
- Next, you will use the docker build command to create a Docker image based on the Dockerfile that you have just created.

The standard name for a Dockerfile is just `Dockerfile` and it's just a plain text file.
Dockerfiles use simple, clean, and clear syntax, which makes them strikingly easy to create and use. They are designed to be self-explanatory, especially because they allow commenting just like a good and properly written application source code.

The instructions in this file are not case-sensitive, however, the good idea is to stick to a convention and write them in uppercase–this will make a Dockerfile more readable. You can write comments in the Dockerfile using the hash symbol #. Docker will skip commented lines unless it contains a parser directive, which we are going to cover in a while. While some commands, such as `MAINTAINER`, can be placed anywhere in a Dockerfile, most of them such as `ADD` must be placed in the order you would like them to be executed when building an image. `CMD` and `ENTRYPOINT` are exceptions–only the last `CMD` or `ENTRYPOINT` instruction in a Dockerfile will have an effect.

![](img/0c87321919fa245e22f056c67ec70003_137_0.png)

The order of instructions in a Dockerfile is important–they will be executed in the same order as in the Dockerfile.

Let's start with the description of possible Dockerfile instructions, one by one.

## Dockerfile instructions

In this section, we are going to cover Dockerfile instructions one by one. Let's begin with the instruction that should be the first one in the Dockerfile we are going to create, the `FROM` instruction.

### FROM

The syntax for the FROM instruction is very simple. It can have three forms. It can be as simple as that:

```
FROM <image>
```

By default, Docker will use the image tagged latest. If you need to build upon different tags, you will need to add the tag name after the name of the image. The FROM instruction will take this form then:

```
FROM <image>:<tag>
```

Last but not least, you can even more precisely pick the image using its digest. As you remember from the previous chapters, images with the same tag can change in time, if their author decides so. If you like to lock your base image to the fixed version, you can specify its digest as the image name. This way your own image will be always built using the specific version of the base image, regardless of tags. To specify base image using digest, use the following syntax of the FROM instruction:

```
FROM <image>@<digest>
```

The FROM instruction tells Docker what will be the base for the image being created. Here you can use your skills of finding images on the Docker Hub, learned in Chapter 5, Finding Images. Now this is important–as it will be starting point–choose wisely. Of course you can pick whatever you want to be base for your own image, it's up to you–but my recommendation would be to always prefer the official repositories. By choosing the official image, you can be pretty sure it will be of high quality, tested, supported, and maintained.

![](img/0c87321919fa245e22f056c67ec70003_138_0.png)

You can use your own images in the FROM instruction.

If a FROM image is not found on your Docker host (on your local machine, for example), Docker will try to find and download it from the Docker image index. Again, the Docker Hub will be picked by default.
