When planning IP addressing in Azure, it is essential to note that Azure reserves five IP addresses for its own use in each subnet range, which cannot be allocated to any resources. These five reserved IP addresses are the first four IP addresses and the last IP address in a range, with the IP address ranges starting at 0, not 1.

The reserved five IP addresses are utilized as follows:

+   - x.x.x.0: This first IP address is used for the network address
+   - x.x.x.1: This second IP address is used for the default gateway
+   - x.x.x.2: This third address is used for Azure DNS
+   - x.x.x.3: This fourth IP address is used for Azure DNS
+   - x.x.x.255: This fifth IP address is used for the broadcast address

You will notice from the preceding list that the first IP assigned to a created VM in the subnet will be the .4 address.

Public IP addressing prefixes can be added to Azure VNets and can be assigned to resources such as network interfaces, VPN gateways, NVAs, and internet-facing load balancers. There are basic- and standard-type stock-keeping units (SKUs).

IPv6 can also be used alongside IPv4 in the same VNet and for the same resource. When a resource such as a VM has requirements for IPv6 and IPv4 communication paths, that is known as a dual stack.

# Azure Extended Network

The Extended Network for Azure capability provides a subnet extension, meaning an on-premises subnet can be stretched into Azure to meet the requirements of specific scenarios, and vice versa. Azure resources can be part of your on-premises local network, akin to making the same IP broadcast domain available in separate locations. It should be noted that this is not necessarily a good practice but may be required for a specific scenario that dictates this requirement.

This is achieved by extending the on-premises Layer 2 network with a Layer 3 overlay network using a Virtual Extensible LAN (VXLAN) solution. The advantage is that when VMs move to Azure, they can retain their existing IPs. Ordinarily, this is not possible as there cannot be overlapping IP address space between the on-premises networks and Azure.