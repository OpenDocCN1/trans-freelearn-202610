Windows LAPS is an important tool for securing local administrator accounts on domain-joined computers. However, it is not the only measure needed to mitigate the risk of abuse of these accounts. Another important step is implementing restrictions and controls, such as those available through Group Policy, to reduce the risk of lateral movement by attackers. As previously discussed, Group Policy can be used to configure settings that limit the capabilities of local administrator accounts and make it more difficult for attackers to use them to move laterally within a network.

There are several settings that should be configured to restrict the use of local administrator accounts. Within these settings, we need to add the **S-1-5-114: NT AUTHORITY\Local account and member of Administrators group** to the following Group Policy by navigating to:

**Computer Configuration | Windows Settings | Security Settings | Local Policies | User Rights Assignment**

+   - Deny access to this computer from the network
+   - Deny log on as a batch job
+   - Deny log on as a service
+   - Deny log on through Terminal Services
+   - Debug

As one example seen in the following screenshot, we have added the account to the **Deny Log on through Remote Desktop Services** policy:

![](bbox=[0.14, 0.54, 0.84, 0.71])

If someone then tries to log on to RDP on a server that has this policy using a local account or using a user from the administrator group, they will see the following error message: