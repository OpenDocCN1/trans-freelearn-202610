
Because we are only going to cover the main points on how to build a secure network in Azure, we will discuss some ideas regarding how you could design your network since many factors apply when setting it up for your organization.

Within Azure, firstly, there are some main concepts and features that you should be aware of:

- **Azure resource group:** This is a logical container within Azure where resources are deployed to. For instance, a virtual machine will consist of different resources such as network cards, a virtual disk, and a virtual machine object. All these resources that are created in Azure need to be part of a resource group.
- **VNet:** This is a software-defined virtual network within Azure. As a customer, you can have one or multiple virtual networks in Azure. These virtual networks are bound to a specific Azure region and have defined address spaces, such as `10.0.0.0/16`.
- **Network Security Group (NSG):** This is a 5-tuple (IP, port, protocol) set of firewall rules that can either be assigned to a subnet or a network interface.
- **NSG flow logs:** This is a feature that allows us to collect log information (in JSON) about traffic flowing through an NSG, regardless of whether it is traffic that is permitted or blocked. This log information can then be exported to different services in Azure, such as Log Analytics, Storage accounts, or Event Hubs.
- **User-Defined Route (UDR):** This is a routing feature that allows you to create a route table and then associate it with one or multiple subnets.
- **VNet peering:** This is a feature that allows you to connect one virtual network with another. This feature will also automatically propagate all routes across each virtual network.
- **Azure Firewall:** This is a managed firewall service in Azure that is running on its own subnet on a virtual network. This firewall service provides different security features such as network rules, application rules, NAT features, TLS inspection, and IPS mechanisms, depending on what kind of SKU is being used.
- **Azure Application Gateway:** This is a load balancer service that operates at the HTTP level, meaning that it can load balance traffic based on content. It also provides a web application firewall mechanism that can be used to protect web applications that are published through the service against known HTTP attack vectors.
- **Azure Defender for Cloud:** This is a cloud-based threat detection service that can provide threat detection against different PaaS services in Azure and IaaS. For instance, it can be used to detect abnormal traffic flow into your environment. For virtual machines, it offers EDR capabilities and the option for vulnerability detection through either Microsoft’s built-in services or Qualys.
- **Azure Bastion:** This provides secure connectivity to virtual machines using RDP or SSH without needing to expose the machines to their public IP addresses. Connectivity is done through the Azure web portal. This service also supports any IP connection, meaning that we can use this against on-premises resources, also providing ease of management.
- **Azure policies:** These allow us to define central organizational policies to ensure that those with access to Azure are not able to provision resources without certain attributes or to ensure that they can only create resources within certain regions. In addition, they can also be used to block certain actions in Azure, such as blocking access so that administrators can't provision virtual machines with a public IP address. This means that we can use Azure policies to ensure the governance of resources.
- **Azure Log Analytics:** This is a centralized log service in Azure that can collect logs/events/metrics from PaaS and IaaS services in Azure. It can also be used to collect log data from other sources on-premises as well. This is the base service that is used for Azure Sentinel.

In addition to this, you have the main orchestration layer within Azure, which is called Azure Resource Manager. All actions that you do either through the Azure portal or a CLI/API/SDK are done via Azure Resource Manager.

Also, to be able to set up any resources in Azure, you need an active subscription, which is a billing account to which resources are linked. Then, you need to have a user account within an Azure AD tenant that has permission to a subscription to be able to provision any resources within Azure.

Within Microsoft Azure, the network flow is quite simple. If you have a virtual network and you provision a virtual machine that is placed within the virtual network, it will, by default, have access to the internet. For each VNet that is provisioned in Azure, Microsoft will automatically assign a hidden public IP to the VNet so that virtual machines can connect to the internet. In addition, all resources that are placed within the same VNet will be able to communicate with each other, since routing is automatically in place for all resources within the same VNet.

This is true unless we have NSG rules in place that prohibit a virtual machine from communicating either externally or with other machines on the same network.

Just setting up a flat virtual network in Azure and placing all resources within that network is not a recommended approach. This means that you have no proper way to segment traffic and that the only way to control or restrict traffic is by using NSG rules, which are quite limited.

The preferred approach is using a hub-and-spoke-based topology, as seen in the following diagram:

![](img/7b49a27a3c2adc52d097f2e77f3bf394_76_0.png)

Figure 2.5 – Hub-and-spoke architecture within Microsoft Azure

The hub-and-spoke architecture is like other zone-based designs, where each spoke is a virtual network acting as a separate zone. The hub is typically a centralized firewall mechanism such as Azure Firewall or some third-party Network Virtual Appliance (NVA) in a virtual network. The hub is also where all internet-bound traffic will be routed and will also host other services that are used to publish external services, such as Azure Web Application Firewall.

All spokes are connected to the hub using VNet peering, and since VNet peering is non-transitive, this means that if VNets A and B are connected and VNets B and C are connected, A and C are still not able to communicate with each other directly. Within each VNet, we will have an associated UDR that points all traffic that is directed to another subnet via the centralized firewall service to allow communication between different spokes.

This approach ensures that all traffic between different zones will be handled by the centralized firewall service, where we can apply traffic detection and blocking. This should be used in combination with NSG rules applied to each subnet to ensure that you have segmentation within each subnet as well.

While there is no one-size-fits all architecture and design, Microsoft has many reference architectures that can be used, depending on your requirements or compliance demands. These can be found at https://docs.microsoft.com/en-us/azure/governance/blueprints/samples/. So, consider using these as starting points when it comes to your secure foundation in Microsoft Azure.

## Summary

In this chapter, we provided an overview of some of the mechanisms that should be part of the security foundation, such as strong authentication mechanisms, identity life cycle management, and vulnerability management. Then, we looked at the different zero-trust pillars, such as identity, device, and network. Lastly, we looked at how security monitoring can help us detect attacks or signs of compromise.

Having this as part of our security foundation can greatly reduce the risk of any type of attack that is utilizing weak security credentials or vulnerabilities.

In the next chapter, we will take a closer look at how to set up security monitoring using Microsoft Sentinel and Microsoft Defender for Cloud to provide better insight into our environment.
