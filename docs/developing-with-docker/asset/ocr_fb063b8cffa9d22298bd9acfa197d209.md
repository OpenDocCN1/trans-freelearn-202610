The output will contain the list of nodes with their hostnames, availability status, and a manager status. Having our little swarm up and running, we can create an overlay network to enable to hosts participating in a swarm to communicate with each other. This will be the same `network create` command we already know; this time, however, we are going to use the overlay driver:

```
docker network create --driver overlay mynet
```

If you now list the networks, you will see `mynet` as an overlay network with a swarm scope, as shown in the following screenshot:

![](bbox=[0.1, 0.3, 0.9, 0.54])

If you are unable to run Docker in the latest version (supporting swarm mode), you can also create multi-host networking. To do this, you will need a key-value store. Let's set it up now.

# Key-value store overlay networking

There is one requirement. In order to create an overlay network, you need to have a key-value store configured: