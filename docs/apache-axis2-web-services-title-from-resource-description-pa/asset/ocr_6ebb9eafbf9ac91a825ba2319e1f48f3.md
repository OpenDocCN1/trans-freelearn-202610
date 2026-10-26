![](bbox=[0.35, 0.15, 0.61, 0.32])

# Increasing horizontal scalability

As shown in the figure below, to achieve horizontal scalability, an Axis2 cluster will be fronted by a load balancer (depicted by LB in the following figure). The load balancer will spread the load across the Axis2 cluster according to some load balancing algorithm. The round-robin load balancing algorithm is one such popular and simple algorithm, and works well when all hardware and software on the nodes are identical. Generally, a horizontally scalable cluster will maintain its response time and will not degrade performance under increasing load. Throughput will also increase when the load increases in such a setup. Generally, the number of nodes in the cluster is a function of the expected maximum peak load. In such a cluster, all nodes are active.

![](bbox=[0.3, 0.55, 0.67, 0.68])