The IP address in the last command is the IP address of the master node. Often you can use `$(docker-machine ip swarm-master)` there. If you are running your hosts on a service such as GCE, you will want to use the internal IP address there as it does not change.
Repeat the process for the third host:

```
$ eval $(docker-machine env swarm-host2)
$ weave launch --ipalloc-init consensus=3
$ weave connect 10.128.0.2
```

Once Weave is installed and running on all three nodes, check the status of the network with weave status:

```
$ weave status
Version: 1.6.1 (up to date; next check at 2016/09/16 23:30:55)
Service: router
Protocol: weave 1..2
Name: 22:b6:71:6d:9c:d2 (swarm-host2)
Encryption: disabled
PeerDiscovery: enabled
Targets: 1
Connections: 2 (2 established)
Peers: 3 (with 6 established connections)
TrustedSubnets: none
Service: ipam
Status: idle
Range: 10.32.0.0/12
DefaultSubnet: 10.32.0.0/12
Service: dns
Domain: weave.local.
Upstream: 169.254.169.254
TTL: 1
Entries: 0
Service: proxy
Address: tcp://104.197.144.79:12375
Service: plugin
DriverName: weave
```

# Connecting containers to Weave

Now that the network is set up, it is time to start containers that use it. The containers will use the weave network that was created when Weave was started. This example will start two containers named shell11 and shell12. The shell11 container will run in the background:

```
$ docker run --net=weave -h shell1.weave.local $(weave dns-args) -tdi weaveworks/ubuntu
```