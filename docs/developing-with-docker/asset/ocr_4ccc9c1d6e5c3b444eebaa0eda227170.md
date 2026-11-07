# 7

# Running Containers

In previous chapters, we have learned about the Docker architecture, networking, persistent storage, and the structure of Dockerfile. At this point, you should be able to create your own Docker image and start using it. Actually, we did run the containers several times, but without going into details; we are going to do that in this chapter. In this chapter, we are going to focus more on running the containerized software. This will include the following concepts:

+   - Container running modes
+   - Overriding the `CMD` and `ENTRYPOINT`
+   - Monitoring containers
+   - Exit codes and restart policies
+   - Runtime constraints on resources
+   - Docker Swarm mode

You will learn how to set up your containers to behave well when running by specifying the restart policy and runtime constraints. In `Chapter 6, Creating Images`, we already discussed the `CMD` and `ENTRYPOINT` commands of Dockerfile. In this chapter, we will focus on how to override them during the container startup, this time in detail. We will learn how to read containers logs and output them not only to the text file, but to some specialized log tools, such as Splunk or Fluentd, for further analysis or processing. At the end of the chapter, we will learn about the new Docker feature – the Swarm mode, which allows you to manage the cluster of your Docker containers.