control/elevate-access-global-admin. So just ensure that you have limited global administrator accounts as well. Use features such as Azure AD administrative units if you want to delegate access within Azure AD.

+   - If you want to use PaaS services in Microsoft Azure in combination with virtual machines, ensure that you are deploying these services using private endpoints. This ensures that PaaS services and connectivity will remain within a virtual network in Azure instead of being publicly exposed.
+   - Ensure that all virtual machines have endpoint protection, Update Management, and Log Analytics/Azure Monitor Agents installed. Secondly, security logs should be collected in a centralized Log Analytics workspace with either Microsoft Sentinel enabled or a third-party SIEM service.
+   - Use services such as Azure Bastion to provide secure remote access to virtual machines for administrators instead of having jump hosts externally available. Azure Bastion also supports per-IP connections, meaning that you can also use it to connect to on-premises resources.
+   - Ensure that you apply Azure policies for your Azure environment so that you reduce the risk of having unsecured services published that are internet-facing or not having adequate security mechanisms in place for the logging of your services. For instance, ensure that services can only be provisioned within approved regions, can only be placed behind centralized security mechanisms, and that logging is enabled.

These are some of the recommendations that you should apply when building workloads in Azure. It is important to note that Azure is constantly changing and therefore you should always re-evaluate whether you are running your Azure environment according to best practices based on the Microsoft cloud security benchmark, which you can read more about here: https://learn.microsoft.com/en-us/security/benchmark/azure/overview?source=recommendations.

# Summary

With more organizations moving workloads to Microsoft Azure, it is vital that we have a solid understanding of the different services and mechanisms available to us to ensure that we have a secure foundation.

In this chapter, we took a closer look at how to design an Azure-based environment and some of the core components that should be in place to ensure that we have control of the virtual machine resources that are provisioned there.

We learned about some of the core services, such as Azure Policy, Azure Update Management, and Azure Backup, and how they can be used to safeguard our virtual infrastructure in Azure.

In the next chapter, we will take a closer look at how we can provide users with secure access to applications and services while ensuring that we are following zero-trust principles and how we can integrate risk and threat information into access management.

Lastly, we will look at different mechanisms that can help us protect traffic flow from various attacks.