# Docker Swarm mode

We have mentioned the Swarm mode briefly in the Chapter 4, Networking and Persistent Storage, when we were learning about multi-host networks. This time, we will look closer to the container clustering features of Docker. The Swarm mode is quite new feature in Docker – it's available in Docker version 1.12 and up. It means, that you can only use this in Linux, which is obvious, but if you run Docker in Windows or Mac, you can use the native Docker for Windows or Docker for Mac application. However, what Swarm is? Let's explain what was the purpose behind including the Swarm in latest Docker versions.

# The purpose

Running a single application in a Docker container is easy. We already did it a number of times. But what about scaling and fail-over? Imagine that you have an application or service that will need to respond swiftly even under heavy load. If a single container cannot handle the load, then you will probably want to run multiple containers. Using Docker Swarm, you can establish and manage a cluster of Docker hosts as a single virtual system. By enabling the cluster, you basically create a cooperative group of Docker hosts If one them fails and is down, other can still serve an application to the user. Having Docker Swarm cluster running, you also have the ability to add or subtract container instances whenever the load changes. And here comes the best part: we don't have to make the decision on which host to start every container on. It will be made automatically by Swarm – in the background, Docker running in swarm mode decides which nodes to start them on. It may seem complex, but in real life running Docker Swarm is as easy as running single Docker containers. If you will get familiar with Docker commands, you will be familiar with Docker Swarm commands as well. Let's get started with explaining some Swarm mode terminology.

# Terminology

There are some key names in Docker Swarm. We need to understand their meaning to easily understand the rest of the chapter and also external resources. Let's list them one by one:

+   - **Swarm:** This is simply a cluster of running Docker containers. It's a pool of Docker hosts that acts a bit like a single large Docker host.
+   - **The Swarm manager:** This is a Docker host on which you have initialized the Swarm. The manager node serves the special purpose: it maintains the cluster state, schedules services and serves swarm mode HTTP API endpoints.