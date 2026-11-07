# 2

# Installing Docker

In this chapter, we will find out how to install Docker on Windows, Mac OS, and Linux operating systems. There will also be step-by-step instructions on how to set up Docker in the cloud and Amazon EC2 will be used as an example. Next, we will run the sample `hello-world` image to verify the setup and check whether everything works fine after the installation process. We are going to cover the following topics in this chapter:

+   - Hardware requirements for running Docker
+   - Installing on Windows
+   - Installing on Mac OS
+   - Installing on Linux
+   - Installing on the Amazon AWS cloud

Docker installation is quite straightforward, but there are some things you will need to focus on to make it run. We will point them out to make the installation process painless.

![](bbox=[0.13, 0.63, 0.2, 0.71])

> It's worth mentioning that Linux is the natural environment for Docker. The Docker engine is built on top of the Linux kernel. To make it run on Windows or Mac OS, the Linux kernel needs to be virtualized.

The Docker Engine could be run on the Mac and MS Windows operating systems by using the lightweight Linux distribution, made specifically to run Docker containers. It runs completely from RAM, weighs just several dozens of megabytes, and boots in a couple of seconds. During the installation of the main Docker package—the Docker Toolbox—the virtualization engine VirtualBox will also be installed by default. Therefore, there are some special hardware requirements for your machine.