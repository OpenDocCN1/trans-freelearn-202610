11. On the **Networking** tab, review the reference information about using an existing network.

* Basics **Networking** Administration Synchronization Security Settings Tags Review + create

Azure AD Domain Services uses a dedicated subnet within a virtual network to hold all of its resources. If using an existing network, ensure that the network configuration does not block the ports required for Azure AD Domain Services to run. Learn more

Figure 2.14 – Review network information

12. Review the information from the **Help me choose the virtual network and address** helper link; then set **Virtual network** or click **Create new** as required.

Virtual network * (new) aadds-vnet Create new

Help me choose the virtual network and address

Figure 2.15 – Choose a virtual network

13. Review the information from the **Help me choose the subnet and NSG** helper link; then, set **Subnet**.

Subnet * (new) aadds-subnet (10.0.0.0/24)

Help me choose the subnet and NSG

Figure 2.16 – Choose the subnet

14. Click Next.

15. On the **Administration** tab, review the information from the **Help me choose AAD DC Admins** helper link; complete the **AAD DC Administrators** group membership as required, or skip and manage later.

* Basics *Networking **Administration** Synchronization Security Settings Tags Review + create

Use these settings to specify which users should have administrative privileges and be notified of problems on your managed domain. Learn more

AAD DC Administrators Manage group membership

Help me choose AAD DC Admins

Figure 2.17 – Choose admin group membership