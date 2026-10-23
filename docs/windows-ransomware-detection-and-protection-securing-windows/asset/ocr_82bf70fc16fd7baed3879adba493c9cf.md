+   - Deny access to this computer from the network
+   - Deny logging on as a batch job
+   - Deny logging on as a service
+   - Deny logging on locally
+   - Deny logging on through Remote Desktop services

This is to ensure that if someone manages to compromise a domain admin account, they cannot use that account to do lateral movement across member servers using RDP or access it directly. It also ensures that someone with a domain admin account does not accidentally log on to other member servers using that account with, for instance, RDP.

> **Note**
> You should also ensure that domain administrator accounts are not synchronized to Azure AD and that you have separate accounts for managing Active Directory and Azure AD.

# Security logging and monitoring

So far, we have looked at different zero-trust design principles and user life cycle management and how to ensure that users are using least-privilege access. Now, let's take a closer look at the foundation of security monitoring, which is logging.

Regardless of how many security mechanisms we have in place, it is always important to have services or tools in place to monitor activities and events within our systems, regardless of whether they are on-premises running within our data center or cloud services. Once we have that monitoring capability in place, we should be able to use the data that has been collected to look for signs of known attacks, abnormal user activity, or unusual traffic.

For instance, to get an overview of what kind of traffic is flowing in and out of a virtual machine and be able to see that in the context of what is going on inside the operating system, you need multiple log sources, such as the following:

+   - Data from the network (NetFlow or other flow logs)
+   - Data from EDR or other tools on the device to view the connection flow to services
+   - Data from security events or application logs

However, when building a security monitoring solution, we cannot just set up and capture all the data and events in our infrastructure since this would generate an extreme amount of data. As an example, NetFlow logs or other raw network data capture will result in an extreme amount of storage usage.