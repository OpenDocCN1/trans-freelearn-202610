![](bbox=[0.25, 0.09, 0.78, 0.34])

Figure 5.3 – Hub-and-spoke network in Microsoft Azure

This topology commonly uses centralized monitoring, management, and connectivity mechanisms such as firewalls and VPN or direct connectivity features. VNet chaining allows for a layered approach to isolation, which many organizations have as a requirement for certain services.

Each of the spoke networks can be defined within its own Azure subscription, ensuring that access to the hub virtual network can be limited to system administrators and different teams can manage their own spoke.

This approach also uses UDRs that are assigned on a subnet level to override the default routing in Azure to ensure that traffic from workloads in the spokes is routed to the centralized firewall.

This means that all traffic going to the internet or even down to on-premises networks needs to go through the centralized hub. This ensures that you have central governance for all network traffic going in and out of your resource in Microsoft Azure.

While it is important to properly design your VNet, the second part is having control of your virtual machine workloads in Azure.

# The anatomy of a VM in Azure

Creating a virtual machine in Azure can be as simple as running two PowerShell commands, but there are considerations that you should be aware of before setting up any machine.