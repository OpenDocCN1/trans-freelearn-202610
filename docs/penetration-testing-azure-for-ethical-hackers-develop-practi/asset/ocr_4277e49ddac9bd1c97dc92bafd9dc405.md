Here is a screenshot of the command and its output. You can see from the screenshot that the module found a VM but the VM does not have an associated managed identity (1). You can also see that the module found another VM that has a privileged managed identity associated with it (2). The module also displays the role assigned to the VM and the role assignment scope (3):

![](bbox=[0.12, 0.22, 0.86, 0.52])

Figure 5.20 – Lava vm_list_privileged module output

In total, the module should list two VMs (assuming you deleted the VM for the last chapter) with privileged role assignments: a Linux VM with the Owner role assignment at the subscription scope and a Windows VM with the Contributor role assignment. Make a note of the name (linuxvm01) and resource group (pentest-rg) of the Linux VM that was found with owner privileges. As we already have a credential with the Contributor role assignment, we will focus on exploiting the VM with the Owner role assignment to obtain its token for privilege escalation.

+   11. Use the vm_rce module to exploit the Run Command functionality as a Contributor. You will need to specify the VM name and resource group as shown here:

```
Lava $> exec vm_rce -rgrp PENTEST-RG -vm_name linuxvm01
```