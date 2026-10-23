Let's take a look at what this diagram entails:

+   1. Production-related data is collected from the automation and control devices in level 2 and below by, for example, a historian collection service on the automation database server.
+   2. The SQL replication broker service (for example, Pi to Pi replication) in the IDMZ will keep databases on the industrial network synchronized with their counterparts on the Enterprise side (this can be unidirectional or bidirectional).
+   3. The production-related data makes it into the databases of systems on the Enterprise side, such as ERP/MES or (Pi) historian.
+   4. Clients that rely on the production data can retrieve (or store it, if bidirectional replication is used) and access this data from the enterprise systems.

We will look at more examples of how the IDMZ separates enterprise and industrial networks, while still allowing cross-access, in the next chapter.

# Enclaves

Another important aspect of the Industrial Zone is enclaves. Enclaves are logical subsections (subnets) within the industrial network. Typically, these subsections are created by grouping a set of related automation equipment (think functionally-, logically-, or geographically-related devices) into its own subnet (VLAN) or physically separated network and connecting it back up to the rest of the industrial network via a firewall.

This setup allows related production equipment to communicate unhindered (at layer 2 speeds) while communication with other enclaves or Level 3 Site Operations is inspected, controlled, and monitored by a firewall and/or NIDS.

# Micro segmentation

Within enclaves, things can be subdivided even more with micro-segmentation. Let's say you have created an enclave based on production lines and that, within those production lines, there are areas/device groups that do not necessarily need to talk to other areas, such as VFDs and HMIs. Those groups of devices or areas of the enclave can then be micro-segmented into their own subnets within the enclave subnet. Any micro-segments that need to be accessed by other segments, enclaves, or L3 Site Operations can use a sub interface on the OT-Firewall or use a Layer 3-capable enclave switch.