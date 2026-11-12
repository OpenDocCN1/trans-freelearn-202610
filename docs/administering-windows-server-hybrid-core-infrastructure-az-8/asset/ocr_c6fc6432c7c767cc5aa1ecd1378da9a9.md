![](bbox=[0.225, 0.095, 0.75, 0.415])

Figure 10.1 – Physical versus VM versus containers

The preceding diagram shows that, in a nutshell, fewer physical servers are required to deliver the same number of apps.

Looking closely, we can see that the traditional approach of having single apps per server meant we needed two physical servers to deliver the two apps. This diagram shows that the same two apps now only need one physical server with virtualization. Still, we needed to purchase, maintain, secure, and support the OS on each VM, which can be quite a cost and operations burden. With containers, however, we deliver the same number of apps, still on the same number of physical servers as virtualization, but we only need one copy of the OS.

To summarize, with containers, we virtualize the software; this allows many of these compute instances (containers) to share the underlying host software resources of a single-host OS. By doing so, we achieve greater efficiency, scale, and agility.

This section introduced containers and compared them to VMs and the traditional physical server model to help you understand the value and benefits of running containers to host and execute your application’s code. In the next section, we will introduce Docker.