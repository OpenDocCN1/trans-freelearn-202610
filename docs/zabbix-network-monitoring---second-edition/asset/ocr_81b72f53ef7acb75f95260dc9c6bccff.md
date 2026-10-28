Note the method we used to reach the items list for a particular host — we used convenience links for host elements, which is a fairly easy way to get there and the reason why we will use **Configuration | Hosts** often.

Back to what we were after, we can see a fairly long list of already existing items. But wait, didn't the Zabbix status screen that we saw in the first screenshot claim there's a single host and no items? That's clearly wrong! Return to **Reports | Status of Zabbix** (or **Monitoring | Dashboard**, which shows the same data). It indeed shows zero items. Now move the mouse cursor over the text that reads **Number of items (enabled/disabled/not supported)**, and take a look at the tooltip:

| Status of Zabbix |
| PARAMETER | VALUE | DETAILS |
| Zabbix server is running | No | localhost:10051 |
| Number of hosts (enabled/disabled/templates) | 39 | 0 / 1 / 38 |
| Number of items (enabled/disabled/not supported) | 0 | 0 / 0 / 0 |
| Number of triggers (enabled/disabled [problem] | 0 | 0 / 0 [0 / 0] |
| Number of users (online) | 2 | 1 |
| Required server performance, new values per second | 0 |  |
*Tooltip: Only items assigned to enabled hosts are counted*

Aha! so it counts only those items that are assigned to enabled hosts. As this example host, Zabbix server, is disabled, it's now clear why the Zabbix status report shows zero items. This is handy to remember later, once you try to evaluate a more complex configuration.

# Creating a host

Instead of using this predefined host configuration, we want to understand how items work. But items can't exist in an empty space — each item has to be attached to a host.

> In Zabbix, a host is a logical entity that groups items. The definition of what a host is can be freely adapted to specific environments and situations. Zabbix in no way limits this choice; thus, a host can be a network switch, a physical server, a virtual machine, or a website.