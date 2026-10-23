
- **Azure AD Privileged Identity Management (PIM):** This feature is used to control access to roles within Azure AD and Azure. One commonly used approach is that if administrators need to perform changes to something in Azure, they will need to elevate access for themselves in PIM before they gain access to those resources.
- **Azure Policy:** This is a service that provides an evaluation of Azure resources compared to business rules; another way to look at it is group policy for Microsoft Azure. By default, if someone has owner access to an Azure subscription, they can create services across any region and all services with public IP addresses if they want to. Azure Policy allows us to define central policies to determine what resources an administrator is allowed to create and where. A typical scenario involves utilizing policies to restrict access to resources that are limited to deployment within a specific region, and prevent any services from being deployed with a public IP address. While this service is more about controlling access to certain resources in Azure and not directly for a virtual machine, Microsoft is pushing heavily with a new feature called Guest Configuration, which allows us to define OS-based policies using Desired State Configuration. This means that we can use Azure Policy to provide a centralized configuration of Azure resources, including the in-guest operating system on virtual machines as well.
- **Network Security Groups (NSGs):** This is a simple five-tuple firewall rule that can be either applied directly to a virtual machine **Network Interface Card (NIC)** or an Azure virtual network subnet.
- **Network Flow Logs and Traffic Analytics:** With NSG flow logs, we will collect all network flow data going through an NSG regardless of whether it is blocked or allowed. By default, these flow logs are collected into a storage account. However, these flow logs can be collected into Log Analytics using a feature called Traffic Analytics. Microsoft will then collect the data and enrich the data with their threat intelligence information before pushing it into a Log Analytics workspace.
- **Azure Application Gateway and Web Application Firewall:** This is a feature that provides load balancing and a layer-7-based firewall for web applications. It is commonly used to publish web services externally. The Web Application Firewall service is used to stop common layer 7 attacks based on OWASP rules.
- **Azure DDoS Protection:** This is a feature that provides DDoS protection against attacks on layers 3 and 4, typically UDP- and TCP-based attacks. An example would be the activist group called Killnet, which carried out a lot of layer 4 DDoS attacks against Norway in June 2022 using TCP SYN flood attacks, which could be stopped using the built-in DDoS Protection feature in Azure. It should be noted that this service is quite costly and is only applicable for services that are publicly available with a public IP address.
- **Azure Disk Encryption:** This is a feature that provides BitLocker encryption for virtual machines, where the encryption mechanism stores the encryption/decryption keys in another service in Azure called Azure Key Vault. This ensures that any virtual machine with content is encrypted so that if someone manages to gain access to the Azure environment, they are not able to download and inspect the contents of the machine.
- **Azure Storage Service Encryption:** This is a feature that encrypts data stored on the physical drives inside Azure data centers and is enabled by default. Since this feature is for physical drives, it does not have any effect on actual running workloads and data.

> **Note**
> 
> Most of the features listed earlier are not enabled by default and involve additional configuration and additional cost. I always recommend looking up the different services and understanding the price of each of them before you start using them, so you do not get any surprises. You can use the Azure pricing calculator, which shows the costs of the different services: https://azure.microsoft.com/en-us/pricing/calculator/.

Now let us group these features together to see how they fit together in a simplified example architecture in Microsoft Azure.

The following diagram illustrates a simple workload where we have a virtual machine within a virtual network in Azure. The machine is using Azure Firewall as a gateway to allow communication with the outside world. The server is running a web service that is exposed externally using Azure Application Gateway, which acts as a reverse proxy.

![](img/7b49a27a3c2adc52d097f2e77f3bf394_160_0.png)

Figure 5.2 – An example architecture for virtual machines in Microsoft Azure

In this example, we have defined NSG rules for both the NIC and the subnet in which the machine resides. The machine is enabled with disk encryption of the OS and is using Microsoft Sentinel to collect security events and EDR and vulnerability detection using Defender.

DDoS protection is enabled and applies only to services that have public IP addresses attached to them. We also have Azure policies attached to the subscription to ensure that administrators cannot deploy any resources within this subscription with public IP addresses. This is the most important part here to ensure that services are not directly exposed.

When you set up a workload or VM in Azure that has a direct public IP address with limited firewall mechanisms in place, it takes only a couple of minutes before you are hit with automated brute force attacks, since the public cloud is a commonly targeted platform. It should be noted that there are some components missing from the architecture shown earlier to ensure that it would work properly, such as route tables and DNS configuration, but we will get back to that.

While this design could work, it does not scale properly from either a networking or a management perspective. To provide some background on this, when we have many virtual machines or resources within a single virtual network by default, they will automatically have route tables in place, which ensures that all resources within the same VNet are able to reach each other directly. To ensure that all traffic passes through the central firewall as the next hop, we make use of a functionality known as User-Defined Routes (UDRs), which overrides the default behavior. This is a fundamental aspect of the hub-and-spoke network architecture.

## Hub-and-spoke virtual networks

When building a scalable network architecture in Azure, many organizations use a common network topology that relies on virtual network chaining, also known as hub-and-spoke network architecture. This topology uses the concept of a connecting group of networks to a virtual network hub like a chain. In this design, as seen in the following diagram, it is easy to add and remove spokes without affecting the rest of the network. This architecture uses a feature called VNet peering to connect the different spokes to the hub. This ensures that traffic from one spoke to another will need to go through the hub before going to another spoke. Since VNet peering is nontransitive, this ensures that spoke one cannot directly communicate with spoke two without going through the centralized firewall, as seen in the following diagram.

![](img/7b49a27a3c2adc52d097f2e77f3bf394_162_0.png)

Figure 5.3 – Hub-and-spoke network in Microsoft Azure

This topology commonly uses centralized monitoring, management, and connectivity mechanisms such as firewalls and VPN or direct connectivity features. VNet chaining allows for a layered approach to isolation, which many organizations have as a requirement for certain services.

Each of the spoke networks can be defined within its own Azure subscription, ensuring that access to the hub virtual network can be limited to system administrators and different teams can manage their own spoke.

This approach also uses UDRs that are assigned on a subnet level to override the default routing in Azure to ensure that traffic from workloads in the spokes is routed to the centralized firewall.

This means that all traffic going to the internet or even down to on-premises networks needs to go through the centralized hub. This ensures that you have central governance for all network traffic going in and out of your resource in Microsoft Azure.

While it is important to properly design your VNet, the second part is having control of your virtual machine workloads in Azure.

## The anatomy of a VM in Azure

Creating a virtual machine in Azure can be as simple as running two PowerShell commands, but there are considerations that you should be aware of before setting up any machine.
