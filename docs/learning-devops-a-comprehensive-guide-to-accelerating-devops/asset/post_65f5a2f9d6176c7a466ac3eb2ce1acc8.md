

## Welcome to nginx!

If you see this page, the nginx web server is successfully installed and working. Further configuration is required.

For online documentation and support please refer to [nginx.org](https://nginx.org/). Commercial support is available at [nginx.com](https://nginx.com/).

_Thank you for using nginx._

Figure 8.9 – The nginx home page

We can now see the home page of nginx.

In this section, we learned how to write pipeline code to execute Terraform and then Ansible automatically with a YAML pipeline in Azure DevOps, and Terraform and Ansible command lines.

## Summary

In this chapter, we implemented a sample pipeline for IaC. In the first section, we learned how to integrate Packer command lines in pipelines. Then, in the second part, we continued the automation of IaC with another pipeline that ran Terraform to provision an Azure VM, and installed nginx on this VM with Ansible.

All the YAML pipeline code used in this chapter was applicable to GitHub and Azure Pipelines, and the process is exactly the same for other CI/CD tools such as Jenkins or GitLab CI.

In the next chapter, we will learn how to build and run a container using Docker.

## Questions

- 1. What is the name of the tool used in this chapter?

- 2. What is the IaC provisioning order between Terraform, Ansible, and Packer?

## Further reading

If you would like to find out more about IaC pipelines, here are some labs:

- DevOps Lab about Terraform on Azure DevOps: `https://azuredevopslabs.com/labs/vstsextend/terraform/`.

- DevOps Lab about Ansible on Azure DevOps: `https://www.azuredevopslabs.com/labs/vstsextend/ansible/`.

## Section 3: Containerized Microservices with Docker and Kubernetes

In this part, we present the basic uses of Docker and how to create and run containers from a Dockerfile. Then, we explore the role of Kubernetes and how to deploy more complex applications on Kubernetes.

This section comprises the following chapters:

- Chapter 9, Containerizing Your Application with Docker
- Chapter 10, Managing Containers Effectively with Kubernetes

## 9 Containerizing Your Application with Docker

In the last few years, one technology in particular has been making headlines on the net, on social networks, and at events—Docker.

Docker is a containerization tool that became open source in 2013. It allows you to isolate an application from its host system so that the application becomes portable, and the code tested on a developer’s workstation can be deployed to production without any concerns about execution runtime dependencies. We’ll talk a little about application containerization in this chapter.

A container is a system that embeds an application and its dependencies. Unlike a **virtual machine (VM)**, a container contains only a light **operating system (OS)** and the elements required for the OS, such as system libraries, binaries, and code dependencies.

To learn more about the differences between VMs and containers, and why containers will replace VMs in the future, I suggest you read this blog article: `https://blog.docker.com/2018/08/containers-replacing-virtual-machines/`.

The principal difference between VMs and containers is that each VM that is hosted on a hypervisor contains a complete OS and is therefore completely independent of the guest OS that is on the hypervisor.

Containers, however, don't contain a complete OS—only a few binaries—but they are dependent on the guest OS, and use its resources (**central processing unit (CPU)**, **random-access memory (RAM)**, and network).

In this chapter, we will learn how to install Docker on different platforms, how to create a Docker image, and how to register it in Docker Hub. Then, we'll discuss an example of a **continuous integration/continuous deployment (CI/CD)** pipeline that deploys a Docker image in **Azure Container Instances (ACI)**. After that, we will show how to use Docker to run tools with **command-line interfaces (CLIs)**.

Finally, we will also learn the basic notions about **Docker Compose** and how to deploy Docker Compose containers in ACI.

This chapter covers the following topics:

- Installing Docker
- Creating a Dockerfile
- Building and running a container on a local machine
- Pushing an image to Docker Hub
- Pushing a Docker image to a private registry (ACR)
- Deploying a container to ACI with a CI/CD pipeline
- Using Docker for running command-line tools
- Getting started with Docker Compose
- Deploying Docker Compose containers in ACI

### Technical requirements

This chapter has the following technical requirements:

- An Azure subscription. You can get a free account here: `https://azure.microsoft.com/en-us/free/`.
- For some Azure commands, we will use the Azure CLI. Refer to the documentation here: `https://docs.microsoft.com/en-us/cli/azure/install-azure-cli`.
- In the last part of this chapter, in the *Creating a CI/CD pipeline for the container* section, we will discuss Terraform and the CI/CD pipeline, which were explained in *Chapter 2, Provisioning Cloud Infrastructure with Terraform*, and *Chapter 7, Continuous Integration and Continuous Deployment*.

All of the source code for the scripts included in this chapter is available here:
https://github.com/PacktPublishing/Learning-DevOps-Second-Edition/tree/main/CHAP09.

Check out the following video to see the code in action:
https://bit.ly/3tlJov8

### Installing Docker

Docker’s daemon is free and very well suited to developers and small teams—it’s what we’ll use in this book.

Docker is a cross-platform tool that can be installed on Windows, Linux, or macOS and is also natively present on some cloud providers, such as **Amazon Web Services (AWS)** and Azure.

To operate, Docker needs the following elements:

- **The Docker client**: This allows you to perform various operations on the command line.
- **The Docker daemon**: This is Docker’s engine.
- **Docker Registry**: This is a public registry (Docker Hub) or private registry of Docker images.

Before installing Docker, we will first create an account on Docker Hub.

### Registering on Docker Hub

Docker Hub is a public space called a **registry**, containing more than 2 million public Docker images that have been deposited by companies, communities, and even individual users.

### Containerizing Your Application with Docker

To register on Docker Hub and list public Docker images, perform the following steps:

1. Go to https://hub.docker.com/, where you will see the following screen:

![](img/d3344b39c888aab0b832625465f837c9_288_0.png)

Figure 9.1 – Docker Hub login page

2. Fill in the form with a unique identifier (ID), an email, and a password. Then, click on the Sign Up button.

3. Once your account is created, you can then log in to the site, and this account will allow you to upload custom images and download Docker Desktop.

4. To view and explore the images available from Docker Hub, go to the Explore section, as indicated in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_289_0.png)

Figure 9.2 – Docker Hub Explore page

A list of Docker images is displayed with a search filter that you can use to search for official images or images from verified publishers, as well as images certified by Docker.

Having created an account on Docker Hub, we will now look at installing Docker on Windows.

### Docker installation

We'll now discuss the installation of Docker on Windows in detail.

Before installing Docker Desktop on Windows or macOS, we need to check all license options. For more information about Docker Desktop licensing, read the pricing page (https://www.docker.com/pricing) and the **frequently asked questions (FAQ)** page (https://www.docker.com/pricing/faq).

To install Docker Desktop on a Windows machine, it is necessary to first check the hardware requirements, which are outlined here:

- Windows 10/11 64-bit with at least 4 **gigabytes (GB)** of RAM
- **Windows Subsystem for Linux 2 (WSL 2)** backend or Hyper-V enabled. You can refer to this documentation in the event of any problems: https://docs.docker.com/docker-for-windows/troubleshoot/#virtualization-must-be-enabled.

