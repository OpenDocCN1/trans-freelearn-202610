![](bbox=[0.164, 0.092, 0.803, 0.497])

Figure 9.8 – docker --help command

As you can see in the preceding screenshot, the command displays the different operations available in the Docker client tool.

Before looking at the execution of Docker commands in detail, it is important to have an overview of Docker’s concepts.

# # An overview of Docker's elements

Before executing Docker commands, we will discuss some of Docker’s fundamental elements, which are **Dockerfiles**, **containers**, and **volumes**.

First of all, it is important to know that a **Docker image** is a basic element of Docker and consists of a text document called a Dockerfile that contains the binaries and application files we want to containerize.

A **Docker registry** is a centralized storage system for shared Docker images. This registry can be public—as in the case of Docker Hub—or private, such as with **Azure Container Registry (ACR)** or JFrog Artifactory.