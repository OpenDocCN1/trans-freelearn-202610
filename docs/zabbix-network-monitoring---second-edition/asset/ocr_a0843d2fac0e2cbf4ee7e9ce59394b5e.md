If a host is required to attach items to, then we must create one. Head over to Configuration | Hosts and click on the Create host button, located in the top-right corner. You will be presented with a host creation screen. This time, we won't concern ourselves with the details, so let's input only some relevant information:

+   - Name: Enter A test host.
+   - Groups: Select Linux servers from the right-hand listbox, named Other groups; press the ◄ button to add this group. Then, select Zabbix servers from the In groups listbox and press the ► button to remove our new host from this predefined group.

> Why did we have to select a group for this host? All permissions are assigned to host groups, not individual hosts, and thus, a host must belong to at least one group. We will cover permissions in more detail in Chapter 5, Managing Hosts, Users and Permissions.

The fields that we changed for our host should look as follows:

![](bbox=[0.07, 0.45, 0.92, 0.68])

When you are ready, click on the Add button at the bottom.

# Creating an item

So, we have created our very own first host. But given that items are the basis of all the data, it's probably of little use right now. To give it more substance, we should create items, so select Linux servers from the Groups dropdown, and then click on Items next to the host we just created, A test host. This host has no items to list—click on the Create item button in the upper-right corner.