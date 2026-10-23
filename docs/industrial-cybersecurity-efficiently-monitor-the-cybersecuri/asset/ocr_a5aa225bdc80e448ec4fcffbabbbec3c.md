# The hardware that's used to build the ICS environment

So, what does it take to build all this?

Although the detailed answer to this question highly depends on the size, functionality, performance, uptime requirements, and geographical dispersity of the ICS, most ICS environments will contain a distinct set of components to make the magic happen. The following diagram is a hardware architecture drawing for an ICS environment:

![](bbox=[0.11, 0.23, 0.88, 0.65])

Figure 2.7 – ICS environment – hardware view

Note that the preceding architecture depicts all the hardware from the IDMZ down. The hardware necessary to host the Enterprise assets of the ICS are part of the Enterprise network and typically manifest in physical computers/servers or virtualized assets (through Hyper-V or VMware vSphere). Also, note that the architecture is only concerned with the hardware necessary to build the industrial network and the IDMZ. The hardware that’s necessary for endpoints such as HMIs, PLCs, DCSs, and so on is not taken into consideration, although some of those assets can be virtualized on the virtualization stack, which is depicted at location 2 in the preceding diagram.