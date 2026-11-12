The second use of drain is to prevent services from being scheduled on manager nodes. Manager processing is very reliant on messages being passed in a timely manner. An unconstrained task running on a manager node can cause a denial of service outage for the node causing problems for your cluster. It is not uncommon to leave manager nodes in a drain state permanently. The following command will drain the node named manager:

```
$ docker node update --availability drain manager
manager
```

| ID | HOSTNAME | STATUS | AVAILABILITY | MANAGER STATUS |
|---|---|---|---|---|
| i13wtadcjz5p509bu313qfbei | worker1 | Ready | Active | Reachable |
| 3f2t4wwthgcahs9089b8przv | worker2 | Ready | Active | Reachable |
| 6d6e6kxlyjuo9vb9w1uug95zh * | manager | Ready | Drain | Leader |

# Activating a node

When a node is ready to accept tasks again, set the state to active. Do not be concerned if the node does not immediately fill up with containers. Tasks are only assigned when a new scheduling event happens, such as starting a service. The following command will reactivate the worker2 node:

```
$ docker node update --availability active worker2
worker2
```

| ID | HOSTNAME | STATUS | AVAILABILITY | MANAGER STATUS |
|---|---|---|---|---|
| i13wtadcjz5p509bu313qfbei | worker1 | Ready | Active | Reachable |
| 3f2t4wwthgcahs9089b8przv | worker2 | Ready | Active | Reachable |
| 6d6e6kxlyjuo9vb9w1uug95zh * | manager | Ready | Active | Leader |

# Removing nodes

Nodes may need to be removed for a variety of reasons including upgrades, failures, or simply eliminating capacity that is no longer needed. For example, it may be easier to upgrade nodes by building new ones rather than performing updates on the old nodes. The new node will be added and the old one removed.

Step one for a healthy node is to set the --availability state to drain. This will ensure that all scheduled tasks have been stopped and moved to other nodes in the swarm.