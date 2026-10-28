Collapse the filter and click on **SNMP device** in the **HOST** column. This will open the host overview page, displaying some basic configuration information. Notably, host interfaces are displayed here. While users without configuration permissions on hosts are not able to open host properties in the configuration section, they may see this host overview page and see the host interfaces this way:

![](bbox=[0.076, 0.196, 0.924, 0.432])

There are also two lines of links at the bottom of this form: **Monitoring** and **Configuration**. As one might expect, they provide quick access to various monitoring and configuration sections for this host, similar to the global search we discussed in *Chapter 2, Getting Your First Notification*. Clicking on **Host name** will provide access to global scripts. We will explore and configure those in *Chapter 7, Acting upon Monitored Conditions*.

Let's return to **Configuration | Hosts** and click on **SNMP device**. Switch to the **Host inventory** tab, and in the **OS** field, enter Linux (`http://www.kernel.org`) and click on **Update**. Let's go directly to **Inventory | Hosts** this time—notice how this was the page we ended at when we clicked on the host count from the inventory overview. Looking at the **OS** column, we can see that Zabbix recognized the URL and made it clickable:

![](bbox=[0.076, 0.72, 0.924, 0.816])

> At this time, the columns displayed on this page cannot be customized.