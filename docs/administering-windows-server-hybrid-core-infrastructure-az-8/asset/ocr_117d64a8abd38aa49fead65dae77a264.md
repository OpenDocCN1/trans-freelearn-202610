We can list role assignments using PowerShell from Cloud Shell using the Get-AzRoleAssignment cmdlet.

Here is an example listing the role assignments for a user:

```
Get-AzRoleAssignment -SignInName smiles@milesbettersolutions.com
```

The preceding code is represented in the following figure:

![](bbox=[0.136, 0.245, 0.866, 0.442])

Figure 13.7 – User role assignments

We can also list user assignments at the resource level by using a -Scope parameter; here is an example:

```
Get-AzRoleAssignment -Scope /subscriptions/8123c407-998a-4ace-afed-360e1839938a/resourceGroups/sm-49-az800-rg/providers/Microsoft.Compute/virtualMachines/sm-49-az800-vm1
```

Here’s the output:

![](bbox=[0.136, 0.631, 0.866, 0.838])

Figure 13.8 – Resource role assignments