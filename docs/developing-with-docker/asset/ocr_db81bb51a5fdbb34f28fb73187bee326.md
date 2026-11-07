# no

The **no** policy is the default restart policy and simply will not restart a container under any case. Actually you do not have to specify this policy, because this is the default behavior. Unless you have some configurable setup to run Docker containers, then the *no* policy can be used as an *off* switch.

# always

If we want the container to be restarted no matter what exit code the command would have, we can use the always restart policy. Basically, it does what it says-Docker will restart the container in every case. The restart policy will always restart the container. This is true even if the container has been stopped before the reboot. Whenever the Docker service is restarted, containers using the always policy will also be restarted, it doesn't matter whether they were executing or not.

![](bbox=[0.12, 0.45, 0.2, 0.56])

With the restart policy, the Docker daemon will always try to restart the container indefinitely.

# on-failure

This is a special restart policy and probably the most often used one. Using the `on-failure` restart policy, you can instruct Docker to restart your container whenever it exits with a non-zero exit status and not restart otherwise. That's why we have begun explaining restart policies with the exit codes. You can optionally provide a number of times for Docker to attempt to restart the container. The syntax of this restart policy is also a little bit different, because using this policy, you can also specify a maximum number of tries that Docker will make to automatically restart the container.

Consider the following example:

```
dockerrun --restart=on-failure:5 mongo
```