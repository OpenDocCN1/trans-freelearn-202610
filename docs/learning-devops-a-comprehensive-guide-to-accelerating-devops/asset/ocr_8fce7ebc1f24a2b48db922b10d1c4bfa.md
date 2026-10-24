In the next section, we will discuss the use of Docker Compose, which allows us to mount several Docker images to the same group of containers.

# # Getting started with Docker Compose

So far in this chapter, we have studied how to write a Dockerfile, create a Docker image, and run a container of this Docker image.

Today, applications are not working in standalone mode; they need other dependencies such as a service (for example, another application; an **application programming interface** (API)) or a database. This implies that for these applications, the Docker workflow is more consistent. Indeed, when we work with several Docker applications, we have to execute for each of them the `docker build` and `docker run` commands, which requires some effort.

Docker Compose is a more advanced Docker tool that allows us to deploy several Docker containers at the same time in the same deployment cycle. Docker Compose also allows us to manage elements that are common to these Docker containers, such as data volumes and network configuration.

> > **Note**
> > For more details about Docker Compose, read the official documentation here:
> > `https://docs.docker.com/compose`

In Docker Compose, this configuration—which contains the Docker images, the volumes, and the network that constitutes the artifacts of the same application—is done simply in a configuration file in **YAML Ain't Markup Language** (**YAML**) format.

In this section, we will learn about the basic mode installation of Docker Compose. Then, we will write a simple Docker Compose configuration file to run an nginx application with a MySQL database in the same context. Finally, we will execute this Docker Compose configuration file and view the result in Docker containers.

# # Installing Docker Compose

On Windows or macOS, the **Docker Compose** binary is already installed with Docker Desktop.

Follow this documentation to install the Docker Compose binary on Linux: `https:// docs.docker.com/compose/install/#install-compose-on-linux- systems`.