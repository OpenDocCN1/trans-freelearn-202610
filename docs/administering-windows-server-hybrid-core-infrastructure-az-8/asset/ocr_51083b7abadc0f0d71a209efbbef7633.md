# # Managing access for Windows Server VMs in Azure

This section looks at the user access controls and network access methods for Windows Server Azure infrastructure-as-a-service (IaaS) VMs.

# ## User access

User access to Azure IaaS VMs for management purposes is controlled through Azure role-based access control (Azure RBAC).

The following are the built-in default compute category roles:

+   * **Virtual Machine Administrator Login**: This lets you manage the VM only. You are unable to access the VM, the virtual network, and any storage account(s) they're connected to
+   * **Virtual Machine Contributor**: This lets you view VMs in the portal and log in as a regular user.
+   * **Virtual Machine User Login**: This lets you view VMs in the portal and log in as an administrator.

These roles are managed via the Access Control (IAM) blade for a VM resource; the following screenshot illustrates the Compute category roles shown in the Azure portal:

![](bbox=[0.125, 0.465, 0.87, 0.685])

# Figure 13.1 - Access control blade

We can use the PowerShell Get-AzRoleDefinition cmdlet to list all the roles:

```
Get-AzRoleDefinition | FT Name, Description | sort
```

The following screenshot example shows a listing of role assignments, formatted into a table showing the name and description in columns and listed alphabetically: