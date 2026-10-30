
`Virtual machine` – Virtual machines run on top of a physical machine, have access to the resources allocated to them, and cannot access more resources than their allocation.

`Hypervisor` – A hypervisor is a program for creating and running virtual machines.

The concept of a virtual machine works seamlessly and is still used in the industry, but it also has a problem with inefficient use of resources. Taking the same analogy, why does every bungalow have its own parking space? They could have a single car park that could be used by residents to park.

Docker solves this problem by letting developers pack their applications into small containers and then the containers can run on the machine independently. Taking the same analogy, Docker can be considered as an apartment building, where tenants stay in each apartment and each tenant can use the amenities. Each apartment has a parking space allocated to them in the parking area. Now we have an efficient way of utilizing the parking area.

Docker provides logical separation between containers, and each container runs independently in its own space without knowing about/accessing the data in other containers (unless specifically shared).

![Figure 11.2: Docker high-level architecture](image/Figure_11.2_B18867.jpg) 

Figure 11.2: Docker high-level architecture

While working with Docker, we come across multiple terms related to Docker. Here are the key concepts of Docker that will help us to understand Docker better:

*   `Dockerfile` – A `Dockerfile` is a textual document providing guidance for constructing a Docker image and detailing the foundational operating system for the container. It outlines necessary elements such as languages, environmental variables, file paths, network ports, and other requirements, as well as the intended actions for when the container is executed.

*   `Docker images` – Docker images encapsulate both the executable application source code and all the tools, libraries, and dependencies essential for the application’s containerized execution. The act of running a Docker image transforms it into either a singular instance or multiple instances of the container.

*   `Docker containers` – A Docker container image serves as a compact, independent, executable software package containing all the essentials for application execution, encompassing code, runtime, system tools, libraries, and configurations.

*   `Docker Engine` – Docker Engine functions as a client-server application, facilitating the processes and workflows necessary for constructing, transporting, and executing applications based on containers. It initiates a server-side daemon process responsible for managing images, containers, networks, and storage volumes.

*   `Docker daemon` – The Docker daemon is the behind-the-scenes service on the host system responsible for overseeing the creation, execution, and distribution of Docker containers. This process operates on the host’s operating system, serving as the interface through which clients communicate.

*   `Docker registry` – The Docker registry acts as the storage hub for Docker images. It serves as a repository, residing either locally for individual users or publicly, such as on Docker Hub, enabling collaborative efforts in application development. Teams, even within the same organization, can seamlessly exchange or distribute containers by uploading them to Docker Hub—a cloud repository akin to GitHub.

*   `Docker Hub` – Docker Hub, the self-proclaimed “world’s largest library and community for container images,” functions as the public repository for Docker images. Boasting a collection exceeding 100,000 container images, it amalgamates contributions from commercial software vendors, open source initiatives, and individual developers. The repository encompasses images crafted by Docker, Inc., certified images from the Docker Trusted Registry, and a multitude of others. Docker Hub users enjoy the freedom to effortlessly share their images and can readily access preconfigured base images to kickstart any containerization endeavor.

*   `Docker Compose` – Docker Compose simplifies the orchestration of multiple containers, treating them collectively as a single service. While executing each container independently, it facilitates seamless interaction and communication among containers.

We will learn more about Docker Compose in the latter part of this chapter (in the *Composing services using docker-compose.yml* section). Now let us learn how to install Docker and use it on our local system.

## Installing Docker

Docker is an open source piece of software available to download for free from the official website. Docker is available for the Linux, Windows, and Mac operating systems. The instructions for installing and running Docker for each platform are well documented and explained in the official guide, hence we shall not give them here. Please follow the instructions given:

*   For macOS users, please follow the steps given at [https://docs.docker.com/desktop/install/mac-install/](https://docs.docker.com/desktop/install/mac-install/).

*   For Windows users, please follow the steps given at [https://docs.docker.com/desktop/install/windows-install/](https://docs.docker.com/desktop/install/windows-install/).

*   For Linux users, please follow the steps given at [https://docs.docker.com/desktop/install/linux-install/](https://docs.docker.com/desktop/install/linux-install/).

Once we have Docker installed, we can run the `docker version` command to verify our installation. *Figure 11**.3* shows the output of all the Docker drivers needed to run Docker properly. *Please note: the version numbers can be different for different platforms and depending on when you are `installing Docker*.

![Figure 11.3: Output of the docker version command](image/Figure_11.3_B18867.jpg) 

Figure 11.3: Output of the docker version command

Once Docker is installed, let us test Docker on our local system.

## Testing Docker on your local system

Now that Docker is installed in our local system, let us try to run a Docker container to verify the Docker setup. Docker has a sample image that we can use to run a container. Docker provides an official image that we can use to test. The `hello-world` image is a simple example that is used by developers to verify the setup.

By running the following commands, we can verify the Docker setup on our local machine:

```
> docker pull hello-world  && docker run hello-world
```

*Figure 11**.4* shows the output we should be able to see when we run the `docker run` command:

![Figure 11.4: The output of the hello-world Docker image](image/Figure_11.4_B18867.jpg) 

Figure 11.4: The output of the hello-world Docker image

Now that we have verified our Docker setup and have run the `hello-world` Docker image, let us learn a few important commands that we need to use.

## Important commands for Docker

The Docker CLI has multiple commands that we can use to interact with and perform different operations. Here we shall look into the most common commands that are used regularly by developers:

*   `docker ps` shows you the list of running containers along with essential information, such as container ID, names, and status. It’s a quick way to check what containers are currently active.

*   `docker pull` downloads a Docker image from a registry.

*   `docker build` builds a Docker image from a `Dockerfile`.

*   `docker run` creates and starts a new container from a specified image. It allows you to customize various aspects of the container, such as port mappings, environment variables, and more.

*   `docker start` starts a stopped container. It brings a container back to life, allowing you to resume its operations.

*   `docker exec` executes a command inside a running container. It’s handy for performing tasks or running applications within a specific container without starting a new one.
