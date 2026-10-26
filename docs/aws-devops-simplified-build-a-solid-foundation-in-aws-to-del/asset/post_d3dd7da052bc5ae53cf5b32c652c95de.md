
## Running Containers in AWS

It’s a well-known fact that traditional monolithic software architectures hosted on-premises slow down teams who want to deliver business outcomes to their customers by leveraging cloud capabilities. So far in this book, we have discussed quite a lot of technical and organizational impacts such applications can have. To overcome these blockers, software teams prefer breaking down the monoliths into smaller manageable components, also known as microservices. These services commonly communicate with each other over HTTP(S), or asynchronous messaging protocols, while offering a unified interface to the end user, as if everything is being managed as a single application. When the right services are used, they are comparatively easy to scale and operate in the cloud. If you are into designing software applications, I would highly recommend going through Characteristics of Modern Microservices Architecture, by Martin Fowler (https://martinfowler.com/articles/microservices.html).

To simplify the adoption of microservice architectures and building applications that are optimized for cloud computing, software teams also use the twelve-factor design pattern (https://12factor.net/). These are time-tested best practices for building modern Software-as-a-Service (SaaS) applications. The 12 factors mainly drive the idea of the following:

- Using *declarative formats* to set up automation
- Having a clean contract with the underlying operating system, offering *maximum portability across environments*
- Building apps that are *suited for deployment on cloud platforms*
- *Scaling up without significant changes* to tooling
- Having *minimum divergence between environments* such as development and production

If I had to call out a single technology that powers all of these areas, it would be **containers**. Microservices and containers go incredibly well together. They empower users to pass on the benefits of the strong application foundations to the end users in the form of frequent and stable business features. In the modern era, containers are the de facto standard for software deployment in the cloud or on-premises. By offering benefits such as low compute and memory footprint, isolation boundaries, and build-once-deploy-anywhere possibilities, containers have emerged as a strong candidate for any greenfield or brownfield application effort. You used the Docker toolbox image to execute all the hands-on exercises in the previous chapters and presumably agree that the consistent experience these containers offer is unmatched. Irrespective of the underlying system configurations, operating systems, or libraries, the hands-on experience for you will be similar. Isn’t this great? These days, it’s unusual for software applications to not have a container-based deployment model. It simplifies a lot of areas that otherwise would have been a pain to manage.

In this chapter, we will further grow our knowledge of containers in AWS by covering the following topics:

- A quick introduction to the container ecosystem
- AWS services that support running containers in the cloud
- Using AWS ECS to deploy a test application stack

## A quick introduction to the container ecosystem

If you’ve not been exposed to containers in your software landscape yet, then this is the introduction for you. I will try to keep it short since there are great resources on the internet if you wish to dive deep into the technology.

### What are containers and why do we need them?

Containers are not a new idea. They have been around for many years in the Solaris, FreeBSD, and Unix operating systems as Solaris Zones, Jails, and Chroot, respectively. In the compute space, we have already experienced a lot of innovations that raise the technical maturity bar while abstracting the underlying complexity. We started with bare-metal servers, moved onto virtual machines for hardware-level virtualization, started using containers for software virtualization, and now have serverless technologies that abstract all of it away from the end user. However, in comparison to serverless technologies, containers are still heavily used due to the balance of maturity and control that they offer. This is often an important requirement for enterprise-scale adoption and usage.

Just like how Java and Python prevent us from having to write assembly-level code, containers abstract a lot of operating system internals to offer benefits such as resource isolation and security boundaries, which ultimately lead to improved agility. Unlike virtual machines, containers are a form of operating system virtualization. They are standard units of software that encapsulate the code, and all of its dependencies, so that the application can be executed reliably, and quickly, on multiple environments.

Docker is an open source container platform that has revolutionized the concept of containers. It offers yet another layer of abstraction and constructs that have now shaped the industry norms when it comes to containerized tooling, applications, and platforms. Let’s discuss the key benefits you get with Docker and the underlying constructs it uses.

## Docker as a container platform

Docker is a portable runtime application environment that allows us to run an application bundled together with its dependencies, on several platforms, with the same end user experience.

The key reason for Docker’s success and industry-wide adoption are the benefits that it helps realize – that is, portability, immutability, faster developments and deployments, and efficient resource utilization. It’s important to reinforce that all these benefits trace back to the *microservices architecture* and *twelve-factor* design methodologies that we discussed at the beginning of this chapter.

### Key concepts used by Docker

To offer a consistent experience to the application developers, some key concepts are used in Docker’s architecture that are worth discussing.

### Docker images

A Docker image is a reusable *read-only template* that includes code, dependencies, runtime, and tools that can be replicated by one or more containers. It is usually based on another parent image and adds some customization delta on top of it. For example, using Ubuntu as a base image, you could add Nginx on top to build an image that offers web server capabilities.

You can create your own `Dockerfile` – a blueprint that contains all instructions for creating Docker images. It specifies the base image to use, the dependencies to install, and the commands to run during the image creation process. Once the Docker image has been built, it can be used to spawn multiple containers.

### Filesystem layers

This is what powers the creation of images and container executions. Every single instruction in your `Dockerfile` results in a new read-only layer being added on top of previous instructions. When a container is spun off from this image, Docker adds an ephemeral thin read-write layer for the specific container instance that can be used by processes running inside that container.

### Docker containers

Containers are the *running instances* of an image. The thin write-only layer they get lasts for the lifetime of the container and can be used by the contained processes for any *transient needs*. The `docker run` command we have used in the book creates a container by pulling the `akskap/awsdevopssimplified-toolbox` image onto your local system (if it doesn’t exist already) and then running a container from the packaged artifact.

We can see how containers, images, and the surrounding tools work together in *Figure 7.1:*

![](img/ecd265dfca9bb64161408ba8a944a4d9_166_0.png)

Figure 7.1 – Docker ecosystem and the relationship between containers and images

Next, let’s briefly discuss what registries are and how they work.

### Docker registries

Registries host Docker images. They can be public or private, depending on how you would like to distribute those artifacts to the end user. Common examples are **Docker Hub** and **Amazon ECR**. Organizations that need to host private registries and encrypt the data at rest can use managed KMS keys and IAM. Under the hood, ECR stores the images in Amazon S3.

By leveraging these capabilities, it’s not very difficult to get started with basic container deployments. However, some areas need more attention.

### Scaling containerized deployments beyond simple use cases

Running one or two container instances on your local system is easy. For CI/CD use cases, teams often run multiple in parallel. This does require some additional development effort on top of the interfaces exposed by respective tools (for example, **GitLab CI/CD** pipeline templates), but it is still manageable. The real challenge, however, comes up when you want to run hundreds and thousands of containers for a production workload. How do you figure out which compute node to place the container on? How do you monitor whether they are still running? How can they securely consume other services such as databases and in-memory caches from the underlying cloud provider? These are the areas that justify the need for a full-fledged container management platform so that you can offload these tasks. These platforms are engineered to solve such problems at scale so that you can focus on writing application code.
