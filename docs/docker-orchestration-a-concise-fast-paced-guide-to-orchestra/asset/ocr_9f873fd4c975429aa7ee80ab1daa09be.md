```
SWMTKN-1-0061dt2qjsdr4gabxryrksqs0b8fnhwg6bjhs8cxzen7tmarbi-89mmok3p
f6dsa5n33fb60tx0m \
172.31.26.152:2377
To add a manager to this swarm, run 'docker swarm join-token manager' and
follow the instructions.
```

Included in the output of the `init` command is the command to run on each worker host. It is also possible to run it from your desktop if you have set your environment to use the worker host. You can save the command somewhere, but you can always get it again by running `docker swarm join-token manager`.

As part of the join process, Docker Swarm will create TLS keys, which will be used to encrypt communication between the managers and the hosts. It will not, however, encrypt network traffic between containers. The certificates are updated every three months. The time period can be updated by running `docker swarm update --cert-expiry duration`. The duration is specified as number of hours and minutes. For example, setting the duration to `1000h` will tell Docker Swarm to reset the certificate every 1,000 hours.

# Managing a swarm

One of the easiest pieces to overlook when it comes to orchestrating Docker is how to manage the cluster itself. In this section, you will see how to manage your swarm including adding and removing nodes, changing their availability, and backup and recovery. In *Chapter 10, Why Stop at Containers? Automating Your Infrastructure*, we will learn about automating node deployment in depth.

# Adding a node

New worker nodes can be added at any time. Install Docker, then run `docker swarm join-token worker` to get the `docker swarm join` command to join the host to the swarm. Once added, the worker will be available to run tasks. Take note that the command to join the cluster is consistent. This makes it easy to add to a host configuration script and join the swarm on boot.