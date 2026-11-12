This is represented in the following diagram:

![](bbox=[0.213, 0.133, 0.753, 0.301])

Figure 6.3 – Azure Extended Network subnet extension

This section looked at how to implement IP addressing in an Azure VNet. In the next section, we will look at implementing VNet routing.

# How to implement VNet routing

As we learned earlier, an Azure VNet is a software-defined representation of on-premises networks. When a VNet is created, an RFC 1918 private address space can be configured, and subnets created for the segmented network design are required. In an earlier example, we saw a / 16 address space that contained / 24 subnets for IP assignment to network interfaces that could be attached to VMs to allow them to communicate within the VNet.

Azure has a set of default system routes that automatically provide the Azure built-in routing capability; this allows VMs in the same VNet to communicate with each other with no specific network configuration required. The default gateway address is pre-defined at . 1 in any subnet, and the system routes determine the next hop paths. These routes cannot be edited or removed, although you can provide custom routes that allow you to override the system routes with more specific routes that take priority over the system routes. These custom routes are referred to as UDRs. UDRs are applied through a route table resource. This provides specific network packet control of where the next hop should be for any packet leaving a subnet where it is not desired to use the default . 1 gateway next hops.

The next-hop type can be one of the following:

+   • Virtual appliance
+   • VNet gateway
+   • VNet
+   • Internet