Let us start by creating a network that our containers will use. Docker Swarm ensures that this network is available to all hosts connected to the swarm node:

```
$ docker network create --driver overlay external
```

The --driver option tells Docker that this network uses the swarm native overlay driver. At the end is the name of the overlay network. In this case, external. This is the name that will be used in service definitions to configure the network that will be used.

The --opt encrypted option is optional. If specified, it tells Docker to encrypt the connections between the containers on different nodes. To do this, Docker Swarm will automatically build IPSEC tunnels between any nodes that have containers connected to that network. The encryption key is automatically updated every twelve hours.

Using an encrypted overlay network is especially helpful if you have a network that spans providers or as a way to simplify inter-container security. For example, MySQL supports using TLS to connect. However, few containers have TLS enabled for MySQL. Using an encrypted overlay gives you the encryption that you might want when talking to the database without having to worry about certificate distribution or if TLS is supported at all.

# Working with DNS-SD

One of the problems with containers is that every time they start, they get a new IP address. That makes it really hard to configure containers that connect to each other. This gets even harder when the containers are running on different hosts. Fortunately, Docker overlay networks have built-in service discovery using DNS. DNS entries are created as part of the swarm services which are described in Chapter 4, Orchestration with Docker Swarm.

Consider the following command:

```
$ docker service create --network external --name nginx --replicas 2 -p 80:80 nginx 8m2vwm3n9w990kh77vf556k7i
```

This command uses Docker Swarm to create a service named nginx with two replicas. When the service starts, Docker creates a DNS record for nginx that resolves to the two containers that were started. Any other container on the network will be able to use the name nginx to connect to the service. Let us create a busybox container to test with:

```
$ docker service create --name shell --network external busybox sleep 6000 191qi81lvvpnah3eqq8cvaxft4n
```

```
$ docker service ps nginx ID NAME IMAGE NODE DESIRED STATE CURRENT STATE ERROR
```