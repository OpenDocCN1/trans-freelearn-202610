In this chapter, when we talk about how to install a replication system, we will not use Docker containers; this is because to install a replication service, we need to shut down the postgresql service on the replica server, and shutting down a service in a Docker environment shuts down the whole container. However, on Docker, shutting down postgresql and restarting it at the same time can be done when the Docker container starts for the first time, so to better understand how to install a replica on a production server, it is better not to use a Docker container (even if on the GitLab repo you can find some containers that you can use). In this chapter, Docker is only used to help the reader who doesn’t want to install a replica server to learn and instead wants to see how physical replication works; for everything concerning the installation part, we will refer not to the paths of the Docker images but to the paths of a Debian server.

# Technical requirements

In the Learn PostgreSQL GitHub repository, you can find three Docker images:

+   - **chapter17_streaming**: Primary/replica asynchronous replication; if you want to use the replica container after starting the container with:
+   
+   ```
+   chapter_17$ bash run-pg-docker.sh chapter17_streaming
+   ```
+   
+   you have to run:
+   
+   ```
+   chapter_17$ bash run-pg-docker_replica.sh chapter17_streaming
+   ```
+   - **chapter17_synchronous**: Primary/replica synchronous replication; if you want to use the replica container after starting the container with:
+   
+   ```
+   chapter_17$ bash run-pg-docker.sh chapter17_synchronous
+   ```
+   
+   you have to run:
+   
+   ```
+   chapter_17$ bash run-pg-docker_replica.sh chapter17_synchronous
+   ```
+   - **chapter17_delayed**: Primary/replica with delayed replication; if you want to use the replica container after starting the container with:
+   
+   ```
+   chapter_17$ bash run-pg-docker.sh chapter17_delayed
+   ```
+   
+   you have to run:
+   
+   ```
+   chapter_17$ bash run-pg-docker_replica.sh chapter17_delayed
+   ```