# Docker as a container platform

Docker is a portable runtime application environment that allows us to run an application bundled together with its dependencies, on several platforms, with the same end user experience.

The key reason for Docker’s success and industry-wide adoption are the benefits that it helps realize – that is, portability, immutability, faster developments and deployments, and efficient resource utilization. It’s important to reinforce that all these benefits trace back to the *microservices architecture* and *twelve-factor* design methodologies that we discussed at the beginning of this chapter.

# Key concepts used by Docker

To offer a consistent experience to the application developers, some key concepts are used in Docker’s architecture that are worth discussing.

# Docker images

A Docker image is a reusable *read-only template* that includes code, dependencies, runtime, and tools that can be replicated by one or more containers. It is usually based on another parent image and adds some customization delta on top of it. For example, using Ubuntu as a base image, you could add Nginx on top to build an image that offers web server capabilities.

You can create your own `Dockerfile` – a blueprint that contains all instructions for creating Docker images. It specifies the base image to use, the dependencies to install, and the commands to run during the image creation process. Once the Docker image has been built, it can be used to spawn multiple containers.

# Filesystem layers

This is what powers the creation of images and container executions. Every single instruction in your `Dockerfile` results in a new read-only layer being added on top of previous instructions. When a container is spun off from this image, Docker adds an ephemeral thin read-write layer for the specific container instance that can be used by processes running inside that container.

# Docker containers

Containers are the *running instances* of an image. The thin write-only layer they get lasts for the lifetime of the container and can be used by the contained processes for any *transient needs*. The `docker run` command we have used in the book creates a container by pulling the `akskap/awsdevopssimplified-toolbox` image onto your local system (if it doesn’t exist already) and then running a container from the packaged artifact.