The following diagram provides a simplistic high-level outline of a Kubernetes cluster [10] to illustrate how it functions, just to set the context for how Cloud Pak for Data leverages it:

![](bbox=[0.121, 0.151, 0.846, 0.538])

Figure 9.2 – Kubernetes architecture overview

There are essentially two types of host nodes – the **Master** and **Compute** (worker) nodes. In some implementations, a node may have both the master and compute roles, but it is highly recommended to maintain separate masters to ensure a better class of reliability. Each node includes some fundamental components [11] such as the Kubelet daemon and functionality to interact with the container runtimes on each node. The master nodes include certain special components, such as the API server and the scheduler.

Kubernetes uses the etcd database for storing configuration information and this database is critical for overall operations. etcd is a key-value distributed store and, for high availability, can work with multiple replicas on different hosts, establishing a quorum. It is recommended that you start with three master nodes and expand when it becomes necessary to support a larger set of applications at scale.