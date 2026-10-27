Have a look at the following diagram:

![](bbox=[0.208, 0.133, 0.757, 0.513])

Figure 2.16 – Gossip connections

All of the members of the Consul cluster, the servers and clients, are swimming in the same pool. This enables the distributed discovery of all members of the pool, their status, and—of course—the status of any services registered with the Consul clients. Although gossip is how all of the agents learn about changes to cluster members as well as services, all information is still sent from the agents to the server nodes via direct RPCs. Remember, any server within the peer set can receive an update and forward this to the leader for quorum commitment and an update to the state machine.

# Changes in latitudes

We've already seen how Consul shares information within a cluster regarding leadership election, changes in service availability, and changes in node availability. In addition to these items, Consul is always aware of the geographic location of each node relative to others within the cluster. This can help with intelligent decisions such as locating the nearest available service instance to the requesting entity. The discovery mechanism utilizes network tomography offered by the Serf library. Yup—not only do they all gossip about each other's state, but they also know each other's location! The singularity is upon us!