brownfield application effort. You used the Docker toolbox image to execute all the hands-on exercises in the previous chapters and presumably agree that the consistent experience these containers offer is unmatched. Irrespective of the underlying system configurations, operating systems, or libraries, the hands-on experience for you will be similar. Isn’t this great? These days, it’s unusual for software applications to not have a container-based deployment model. It simplifies a lot of areas that otherwise would have been a pain to manage.

In this chapter, we will further grow our knowledge of containers in AWS by covering the following topics:

+   - A quick introduction to the container ecosystem
+   - AWS services that support running containers in the cloud
+   - Using AWS ECS to deploy a test application stack

# A quick introduction to the container ecosystem

If you’ve not been exposed to containers in your software landscape yet, then this is the introduction for you. I will try to keep it short since there are great resources on the internet if you wish to dive deep into the technology.

# What are containers and why do we need them?

Containers are not a new idea. They have been around for many years in the Solaris, FreeBSD, and Unix operating systems as Solaris Zones, Jails, and Chroot, respectively. In the compute space, we have already experienced a lot of innovations that raise the technical maturity bar while abstracting the underlying complexity. We started with bare-metal servers, moved onto virtual machines for hardware-level virtualization, started using containers for software virtualization, and now have serverless technologies that abstract all of it away from the end user. However, in comparison to serverless technologies, containers are still heavily used due to the balance of maturity and control that they offer. This is often an important requirement for enterprise-scale adoption and usage.

Just like how Java and Python prevent us from having to write assembly-level code, containers abstract a lot of operating system internals to offer benefits such as resource isolation and security boundaries, which ultimately lead to improved agility. Unlike virtual machines, containers are a form of operating system virtualization. They are standard units of software that encapsulate the code, and all of its dependencies, so that the application can be executed reliably, and quickly, on multiple environments.

Docker is an open source container platform that has revolutionized the concept of containers. It offers yet another layer of abstraction and constructs that have now shaped the industry norms when it comes to containerized tooling, applications, and platforms. Let’s discuss the key benefits you get with Docker and the underlying constructs it uses.