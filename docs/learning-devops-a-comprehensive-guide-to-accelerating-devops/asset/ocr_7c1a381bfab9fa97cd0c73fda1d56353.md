# # 9 Containerizing Your Application with Docker

In the last few years, one technology in particular has been making headlines on the net, on social networks, and at events—Docker.

Docker is a containerization tool that became open source in 2013. It allows you to isolate an application from its host system so that the application becomes portable, and the code tested on a developer’s workstation can be deployed to production without any concerns about execution runtime dependencies. We’ll talk a little about application containerization in this chapter.

A container is a system that embeds an application and its dependencies. Unlike a **virtual machine (VM)**, a container contains only a light **operating system (OS)** and the elements required for the OS, such as system libraries, binaries, and code dependencies.

To learn more about the differences between VMs and containers, and why containers will replace VMs in the future, I suggest you read this blog article: `https://blog.docker.com/2018/08/containers-replacing-virtual-machines/`.