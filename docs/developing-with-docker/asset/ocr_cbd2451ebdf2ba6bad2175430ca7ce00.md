The preceding command will run MongoDBimage and will try to restart it five times in case of failure before giving up. The main benefit of a on-failures restart policy is that when an application exits with a successful exit code (that means there were no error in the application, it just finished executing) the container will not be restarted. The number of restart tries for a container can be obtained via the docker inspect command. For example, to get the number of restarts for container with specific ID or name:

```
docker inspect -f "{{ .RestartCount }}"<ContainerID>
```

You can also get the last time the container was started again:

```
docker inspect -f "{{ .State.StartedAt }}"<ContainerID>
```

You should know that Docker uses a delay between restarting the container, to prevent flood-like protection. This is an increasing delay, it starts with the value of 100 milliseconds, and then Docker will double the previous delay. In effect, the daemon will wait for 100 ms, then 200 ms, 400ms, 800ms, and so on, until either the on-failure limit is reached or when you stop the container using docker stop or execute the force removal by executing the docker rm -f command.

![](bbox=[0.12, 0.48, 0.2, 0.58])

If a container is successfully restarted the delay is reset to the default value of 100 milliseconds.

# unless-stopped

Again, similar to always if we want the container to be restarted regardless of the exit code, we can use unless-stopped. The unless-stopped restart policy acts the same as always with one exception, it will restart the container regardless of the exit status, but do not start it on daemon startup if the container has been put to a stopped state before. This means that with unless-stopped restart policy is that if the container was running before the reboot, the container would be restarted once the system restarts. When an application within a Docker container exits, that container is halted. If an application that is running within a container crashes, the container stops and that container will remain stopped until someone or something restarts it.