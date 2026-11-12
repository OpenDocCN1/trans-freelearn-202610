This can be thought of as an any-to-any-type connectivity solution.

To implement an Azure virtual WAN, the high-level steps are as follows:

+   1. Create a Virtual WAN resource.
+   2. Create a Virtual WAN hub.
+   3. Create a Virtual WAN site.
+   4. Connect spoke endpoints to the hub.
+   5. Download a configuration file.
+   6. Validate connectivity through the hub and spoke endpoints.

In this section, we looked at Azure Virtual WAN and concluded the topic of network connectivity in Azure. The following section will look at implementing network protection in Azure.

# # Implementing network protection in Azure

This section will look at the network protection options available in Azure. This will cover NSGs, **application security groups (ASGs)**, and the Azure Firewall; we will look at the use cases, how they work, and their capabilities.

This section will look at NSGs as the first skill area.

# ## NSGs

An NSG is a *packet filter* approach controlling traffic flow into and out of resources such as VMs.

A set of **inbound** and **outbound** rules filters network traffic, denying all traffic unless *explicitly allowed*.

An NSG evaluates whether access is *allowed* or *denied* based on five data points (the 5-tuple method); these data points are as follows:

+   * Source
+   * Source port
+   * Destination
+   * Destination port
+   * Protocol