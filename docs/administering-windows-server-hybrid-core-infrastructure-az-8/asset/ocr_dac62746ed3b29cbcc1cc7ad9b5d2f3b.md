Microsoft provides two peering types, as follows:

+   - Private peering: This type provides connections to Azure VNets
+   - Microsoft peering: This type provides connections to Microsoft public services such as Microsoft Office 365 (M365) and Dynamics 365 (D365)

You may have a Site-to-Site VPN and ExpressRoute on the same VNet gateway, but you must use a route-based VPN to support this. It is recommended to allocate a /27 subnet to ensure adequate IPS can be assigned.

In this section, we looked at ExpressRoute. In the next section, we will look at Azure Network Adapter.

# Azure Network Adapter

Windows Server 2019 introduced Azure Network Adapter as a new hybrid connectivity solution. To connect to networks and VMs running in Azure, there is the option of VPNs and ExpressRoute, which can add significant complexity to what is often a simple need to connect resources on-premises to resources in Azure.

With Azure Network Adapter and through Windows Admin Center, you can provide a one-click solution to connect your Azure VNets to on-premises networks using a Point-to-Site VPN.

# Azure Relay

Azure Relay is the service that used to be called Service Bus Relay.

Azure Relay is a hybrid solution that provides the ability to expose internal application resources to public cloud environments. It works based on *listeners* and does this without the need for inbound firewall ports to be open and without intrusive network infrastructure changes to the on-premises environment. This is a different solution from VPNs, which work at the network integration level. This is like an application VPN where we can scope connection access to a single on-premises machine's application rather than a VPN (whose scope is very wide to a network range) and is more about connecting networks than connecting applications.

# Azure Virtual WAN

Azure Virtual WAN provides a **global network transit architecture** solution.

It is a virtualized representation of a **Wide Area Network (WAN)** that would traditionally be in place on-premises, such as an MPLS solution. In the case of Virtual WAN, Microsoft now becomes your connectivity provider.

In essence, the solution is a scaled-out, any-to-any connection type VNet gateway solution. It can handle multiple connection types, such as **P2S**, **S2S**, **ExpressRoute**, and **Software-Defined WAN (SD-WAN)**.