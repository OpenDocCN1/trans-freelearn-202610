
This command displays a list of Docker contexts and indicates with a * symbol the current context, as illustrated in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_324_0.png)

Figure 9.41 – List of Docker contexts

5. Select the newly created demobookaci context by running the following command:

```
docker context use demobookaci
```

6. Finally, to deploy the Docker Compose configuration application inside this ACI resource, run the following command:

```
docker compose up
```

The following screenshot shows the result of the execution of this command:

![](img/d3344b39c888aab0b832625465f837c9_324_1.png)

Figure 9.42 – docker compose up command for deployment to ACI

In our Azure subscription, we can see the created ACI resource with two containers, as indicated in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_324_2.png)

Figure 9.43 – ACI containers created by Docker Compose

Finally, to test and gain access to the deployed application, find the application FQDN in the ACI properties and launch the application in the browser. This is exactly what we learned in the *Deploying a container to ACI with a CI/CD pipeline* section of this chapter.

> Note
> For another sample of Docker Compose in ACI, read an official tutorial here:
> https://docs.microsoft.com/en-us/azure/container-instances/tutorial-docker-compose

In this section, we have learned how to deploy multiple containers using Docker Compose in ACI using the `docker-compose` YAML file and some Docker command-line contexts.

## Summary

In this chapter, we presented Docker and its essential concepts. We discussed the necessary steps to create a Docker Hub account, and then we installed Docker locally with Docker Desktop.

We created a Dockerfile that details the composition of a Docker image of a web application, and we also looked at the principal instructions that it is composed of—`FROM`, `COPY`, and `RUN`.

We executed the `docker build` and `docker run` commands to build an image from our Dockerfile and execute it locally, and then pushed it to Docker Hub using the `push` command.

In the second part of this chapter, we implemented and executed a CI/CD pipeline in Azure Pipelines to deploy our container in an ACI resource that was provisioned with Terraform. Then, we discussed the use of Docker for running command-line tools such as Terraform.

Finally, we learned to install and use Docker Compose to create multiple application containers and deploy them in ACI.

In the next chapter, we will continue with the subject of containers, and we will look at the use of Kubernetes, which is a tool to manage containers on a large scale. We will use the **Azure Kubernetes Service (AKS)** and Azure Pipelines to deploy an application in Kubernetes with a CI/CD pipeline.

## Questions

- 1. What is Docker Hub?
- 2. What is the basic element that allows you to create a Docker image?
- 3. In a Dockerfile, what is the instruction that defines a base image to use?
- 4. Which Docker command allows you to create a Docker image?
- 5. Which Docker command allows you to instantiate a new container?
- 6. Which Docker command allows you to publish an image in Docker Hub?

## Further reading

If you want to know more about Docker, here are some great books:

- *Docker Cookbook*: https://www.packtpub.com/virtualization-and-cloud/docker-cookbook-second-edition
- *Beginning DevOps with Docker*: https://www.packtpub.com/virtualization-and-cloud/beginning-devops-docker

## 10 Managing Containers Effectively with Kubernetes

In the previous chapter, we learned in detail about containers with Docker, about the construction of a Docker image, and about the instantiation of a new container on the local machine. Finally, we set up a **continuous integration/continuous deployment** (CI/CD) pipeline that builds an image, deploys it in Docker Hub, and executes its container in **Azure Container Instances** (ACI).

All this works well and does not pose too many problems when working with a few containers. But in so-called **microservice applications**—that is, applications that are composed of several services (each of them is a container), we will need to manage and orchestrate these containers.

There are two major container orchestration tools on the market: Docker Swarm and Kubernetes.

For some time now, Kubernetes, also known as **K8S**, has proved to be a true leader in the field of container management and is therefore becoming a _must_ for the containerization of applications.

In this chapter, we will learn how to install Kubernetes on a local machine, as well as an example of how to deploy an application in Kubernetes, both in a standard way and with Helm. We will learn in more depth about Helm by creating a chart and publishing it in a private registry on **Azure Container Registry (ACR)**.

Then, we will talk about **Azure Kubernetes Service (AKS)** as an example of a Kubernetes cluster, and finally, we will learn how to monitor applications and metrics in Kubernetes.

This chapter will cover the following topics:

- Installing Kubernetes
- A first example of Kubernetes application deployment
- Using Helm as a package manager
- Publishing a Helm chart in a private registry (ACR)
- Using AKS
- Creating a CI/CD pipeline for Kubernetes with Azure Pipelines
- Monitoring applications and metrics in Kubernetes

## Technical requirements

This chapter is a continuation of the previous chapter on Docker, so to understand it properly, it is necessary to have read that chapter and to have installed Docker Desktop (for the Windows **operating system (OS)**).

In the CI/CD part of this chapter, you will need to retrieve the source code that was provided in the previous chapter on Docker, which is available at `https://github.com/PacktPublishing/Learning-DevOps-Second-Edition/tree/main/CHAP09/appdocker`.

For the section about Helm charts on ACR, it's necessary to have an Azure subscription (register for free here: `https://azure.microsoft.com/en-us/free/`) and that you have installed the Azure **Command-Line Interface (CLI)** binary, available here: `https://docs.microsoft.com/en-us/cli/azure/install-azure-cli`.

The entire source code for this chapter is available at `https://github.com/PacktPublishing/Learning-DevOps-Second-Edition/tree/main/CHAP10`.

Check out the following video to see the Code in Action:
https://bit.ly/3p7ydjt

## Installing Kubernetes

Before installing Kubernetes, we need to have an overview of its architecture and main components, because Kubernetes is not a simple tool but is a cluster—that is, it consists of a **master** server and other slave servers called **nodes**. I suggest you explore the architecture of Kubernetes in a simplified way.

## Kubernetes architecture overview

Kubernetes is a platform that is made up of several components that assemble together and extend on demand, in order to enable better scalability of applications. The architecture of Kubernetes, which is a client/server type, can be represented simply, as shown in the following diagram:

![](img/d3344b39c888aab0b832625465f837c9_329_0.png)

In the previous diagram, we can see that a cluster is made up of a master component and nodes (also called **worker** nodes), which represent the slave servers. In each of these nodes, there are **pods**, which are virtual elements that will contain containers and volumes. Put simply, we can create one pod per application, and it will contain all the containers of the application. For example, one pod can contain a web server container, a database container, and a volume that will contain persistent files for images and database files.

Finally, `kubectl` is the client tool that allows us to interact with a Kubernetes cluster. With this, we have the main requirements that allow us to work with Kubernetes, so let’s look at how we can install it on a local machine.

## Installing Kubernetes on a local machine

When developing a containerized application that is to be hosted on Kubernetes, it is very important to be able to run the application (with its containers) on your local machine, before deploying it on remote Kubernetes production clusters.

In order to install a Kubernetes cluster locally, there are several solutions, which are detailed next.

The first solution is to use **Docker Desktop** by performing the following steps:

- 1. If we have already installed Docker Desktop, which we learned about in _Chapter 9, Containerizing Your Application with Docker_, we can activate the **Enable Kubernetes** option in **Settings** on the **Kubernetes** tab, as shown in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_330_0.png)

Figure 10.2 – Enabling Kubernetes in Docker Desktop
