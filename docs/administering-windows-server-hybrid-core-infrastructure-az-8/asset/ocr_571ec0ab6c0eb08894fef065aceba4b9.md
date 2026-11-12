# Azure AD tenant and subscription

To create Azure resources, you will need an **Azure subscription**. This will act as a logical container, access control, and billing mechanism for the resources created and consumed. You must associate an Azure AD tenant with this subscription; this tenant can be synchronized with AD for a hybrid identity scenario or a cloud-only identity approach.

The Azure AD tenant can only have one Azure AD DS managed domain. Ensure you plan adequately first, as once you have created the managed domain; you cannot change any of the following fields: `tenant`, `subscription`, `region`, `resource group`, `VNet`, or `subnet`.

# Privileges to create an Azure AD DS managed domain

You should ensure you have the following privileges for an account you wish to use to create Azure AD DS managed domains:

An account with **Owner** or **Contributor** rights at the subscription level or an account with **Domain Services Contributor**, **Groups Administrator**, and **Application Administrator** Azure AD roles.

# Network and DNS

The default **virtual network (VNet)** created through the deployment wizard for Azure AD DS uses a **dedicated Azure AD DS subnet** named `aadds-vnet`. A single subnet is only created within the VNet named `aadds-subnet`. This subnet should not be used for anything other than the managed domain resources; do not add any workloads to this subnet.

VMs or resources you create should be in their own **workload subnet** in the `aadds-vnet`; alternatively, you can create another **workload VNet** and connect it to the Azure AD DS VNet using VNet peering.

The following figure outlines both approaches to the network topology:

![](bbox=[0.212, 0.648, 0.755, 0.885])

Figure 2.5 – Azure AD DS network topology