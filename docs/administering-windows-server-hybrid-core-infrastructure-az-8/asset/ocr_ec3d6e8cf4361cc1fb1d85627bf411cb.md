# Deploying a DC from media

This is only for deploying additional DCs to an existing AD DS environment. This deployment approach creates a backup of AD DS and then transfers it to removable media such as a USB drive. You start by deploying AD DS on the server that will be the DC via the Server Manager GUI and select the Install from media option. This is very similar to a non-authoritative recovery – that is, the AD DS database and its objects will only be as current as the date when the backup was taken. So, to get the latest changes to the directory since the backup, pull replication must occur. This will ensure that the new DC is consistent with all the other DCs and has the same copy of the directory objects.

# Deploying a DC in Azure as IaaS

As we learned earlier in this chapter, as well as deploying DCs to on-premises platforms, we can also deploy DCs to the Microsoft Azure public cloud platform as IaaS VMs. However, note that deploying AD DS is not covered here and will be covered in Chapter 2, Implementing and Managing Azure Active Directory Domain Services.

When deploying a DC in Azure or the AD DS role in an IaaS VM, there are some considerations you need to be aware of:

+   * **Networks**: To add DCs to an existing AD DS environment, you should consider network connectivity. Here, you must have a line of sight to those existing DCs. This will require your networks that have DCs placed on them to be extended into Azure. This is typically done over a VPN or, in some scenarios, ExpressRoute.
+   * **Sites/Subnets**: These should be created to reflect the address space(s) defined in your Azure VNet.
+   * **DNS**: Azure AD DNS cannot be used for AD DS; you should use the DNS role within Windows Server or Azure private DNS zones.
+   * **VMs**: Due to the burstable CPU capability in the B Series VMs, they are an excellent fit to be used for DCs.
+   * **Disks**: Since implementing AD DS will install a database on the VM; we should follow some guidance on that scenario – that is, we shouldn’t install on any disks that have caching enabled – especially write caching. To meet this guidance, you should not place the NTDS .DIT and SYSVOL files in the default path offered to you during the configuration wizard; instead, you should attach a data disk to the VM (which has to write caching off by default) and change the path to store these files in the data drive.

# Deploying an RODC

To deploy an RODC, we can use a pre-staged account to perform the AD DS install tasks. This will ensure that a user does not need to be a member of the domain’s admin group or enterprise admins group.

A new site and subnet should be created to control this traffic replication to the RODCs. The replication schedule can be set on the default site link.