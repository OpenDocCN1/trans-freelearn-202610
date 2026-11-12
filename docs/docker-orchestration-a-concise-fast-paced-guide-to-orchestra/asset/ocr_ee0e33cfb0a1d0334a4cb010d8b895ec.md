After you have reviewed the script, run it:

```
$ sh install-docker.sh
```

The user that runs the install script needs to be root or have the ability to use su or sudo.

If you are running a supported Linux distribution, the script will prepare your system and install Docker. Once installed, Docker will be updated by the local package system, such as `apt` on Debian and Ubuntu or `yum` on CentOS and **Red Hat Enterprise Linux (RHEL)**. The `install` command starts Docker and configures it to start on system boot.

A list of supported operating systems, distributions, and cloud providers is located at https://docs.docker.com/engine/installation/.

By default, anyone using Docker locally will need root privileges. You can change that by adding them to the `docker` group which is created by the install packages. They will be able to use Docker without root, starting with their next login.

# Installing with Docker Machine

Docker provides a very nice tool to facilitate deployment and management of Docker hosts on various cloud services and Linux hosts called **Docker Machine**. Docker Machine is installed as part of the Docker Toolbox but can be installed separately. Full instructions can be found at https://github.com/docker/machine/releases/.

Docker Machine supports many different cloud services including AWS, Microsoft Azure, and GCE. It can also be configured to connect to any existing supported Linux server. The driver `docker-machine` uses is defined by the `--driver` flag. Each driver has its own specific flags that control how `docker-machine` works with the service.

# Starting a host on AWS

AWS is a great way to run Docker hosts and `docker-machine` makes it easy to start and manage them. You can use the **Elastic Load Balancer (ELB)** to send traffic to containers running on a specific host or load balance among multiple hosts.