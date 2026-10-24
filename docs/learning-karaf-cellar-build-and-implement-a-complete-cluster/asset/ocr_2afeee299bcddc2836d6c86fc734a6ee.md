Cellar wraps the Hazelcast instance in an OSGi way; this means that the Hazelcast instance is exposed as an OSGi service and can be used by any client bundle. For communication between the nodes, Cellar uses the Hazelcast TCP/IP layer with Java NIO.

Cellar automatically installs a Hazelcast configuration file in `etc/hazelcast.xml`. The Hazelcast configuration and tuning is stored in this file. This chapter covers the contents of this configuration file.

# Distributed cluster resource states

Cellar uses Hazelcast to maintain and store the states of the different resources in the cluster (bundles, features, configuration, OBR, eventadmin, DOSGi, and so on). To do so, Cellar uses Hazelcast's distributed data structures. Thanks to this, the cluster data is distributed across all the nodes in the cluster. Each Cellar node stores a portion of the data (and eventually the replicas/backups).

For instance, in Cellar, you have the following three features installed:

+   - Feature A
+   - Feature B
+   - Feature C

The statuses of the features are stored in a distributed map that looks similar to the following statuses:

+   - Feature A: Installed
+   - Feature B: Uninstalled
+   - Feature C: Installed

The partition of the features' distributed map (in the cluster) can be as follows:

+   - Feature A's status on node1
+   - Feature B's status on node2
+   - Feature C's status on node1

If a node goes down or leaves the cluster, Hazelcast redistributes the data (including the replica) on the remaining nodes. Similarly, when a new node joins the cluster, Hazelcast redispatches the data on the different nodes.

Thus, there is no cluster manager or a single point of failure: each node in the cluster acts the same as the next.