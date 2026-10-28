For more information, look at the following search head cluster architecture:

![](bbox=[0.18, 0.16, 0.82, 0.55])

Figure 7.2: Search head cluster architecture

In Figure 7.2, there is a cluster consisting of three search head instances. These instances are fronted by a load balancer, which evenly distributes the load between the search head members in the cluster. Among the search head instances, one of them is elected to perform the role of the search head captain, which is responsible for coordinating various tasks within the cluster.

The search head captain instance handles tasks such as job scheduling, configuration replication, artifact replication, alert tracking, and knowledge bundle replication across the search head members. However, the captaincy role is not fixed to a specific instance. Instead, an ongoing election process occurs among the search head members to determine the winning member that will take over the captaincy role. This ensures that the responsibilities are dynamically assigned and can be redistributed as needed.

If it is necessary to manually transfer the captaincy from one instance to another, it is possible to initiate the transfer through a CLI command. This allows flexibility and control over the captaincy assignment within the cluster.