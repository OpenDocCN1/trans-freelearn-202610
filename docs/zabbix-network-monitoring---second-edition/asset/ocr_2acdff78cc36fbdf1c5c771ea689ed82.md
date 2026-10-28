# Managing Hosts, Users, and Permissions

Right, we forgot to add the **snmptraps** host. Move your mouse cursor over it—notice how this (and every other host on this page) is a link. Clicking on it will take you to host details, so do that now. As we can see on the host editing form, it is already in one group: **SNMP devices**. Click on **Test group** in the **Other groups** listbox, and then click on the ◀ button:

![](bbox=[0.076, 0.197, 0.902, 0.422])

When you are done, click on **Update**.

You have probably guessed by now that a host can belong to any number of groups. This allows you to choose grouping based on any arbitrary decision, such as having a single host in groups called **Linux servers**, **Europe servers**, and **DB servers**.

Now, we are back in the host list, so return to the host group list by navigating to **Configuration | Host groups**. **Test group** contains four hosts, as it should. Let's say you want to disable a whole group of hosts, or even several host groups. Perhaps you have a group of hosts that are retired but which you don't want to delete just yet, or maybe you want to disable hosts created for testing when creating an actual production configuration on the Zabbix server. The group listing provides an easy way to do that: mark the checkboxes next to the **Linux servers** and **SNMP devices** entries, click on the **Disable hosts** button at the bottom of the list, and confirm the popup.

After this operation, all green hosts should be gone—they should be red now, indicating that they are in a disabled state.

This time, you could also have only marked the checkbox next to **Test group**, as **Linux servers** and **SNMP devices** are subsets of **Test group**, and the final effect would be the same. After doing this, we should remember that **snmptraps** is a generic SNMP trap-receiving host, which probably should be left enabled. Again, click on it to open the host details editing page.