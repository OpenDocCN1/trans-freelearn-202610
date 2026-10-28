We used positional variables in the item name again — if we wanted to monitor another process, it would be easy to clone this item and change the process name in the item key only.

When done, click on the **Add** button at the bottom. Check this item in the **Latest data** page:

| NAME | LAST CHECK | LAST VALUE |
|---|---|---|
| **Zabbix process busy rates** (1 Item) |  |  |
| Zabbix busy process poller rate | 2015-12-11 08:52:51 | 0.69 % |

Most likely, our small Zabbix instance is not very busy polling values. By default, there are 5 pollers started, and they are dealing with the current load without any issues.

As an exercise, monitor a few more process types — maybe trapper and unreachable pollers. Check the Zabbix manual section on internal items for the exact process names to be used in this item.

After adding a few more items, you will probably observe that there are a lot of internal processes. We discussed creating such items automatically using XML importing or the API, but then there were also all the caches we could and should monitor. Zabbix tries to help here a bit and ships with default internal monitoring templates. In the search box in the upper-right corner, enter `app zabbix` and hit the *Enter* key. Look at the **Templates** block:

**Templates**

| TEMPLATE | APPLICATIONS | ITEMS | TRIGGERS | GRAPHS | SCREENS | DISCOVERY | WEB |
|---|---|---|---|---|---|---|---|
| Template App Zabbix Agent | Applications 1 | Items 3 | Triggers 3 | Graphs | Screens | Discovery | Web |
| Template App Zabbix Proxy | Applications 1 | Items 21 | Triggers 19 | Graphs 4 | Screens 1 | Discovery | Web |
| Template App Zabbix Server | Applications 1 | Items 30 | Triggers 26 | Graphs 5 | Screens 1 | Discovery | Web |

Displaying 3 of 3 found

While the agent template is quite simple and not of much interest at this time, the server and proxy templates cover quite a lot, with 31 and 21 items respectively. These templates will allow out-of-the-box monitoring of internal process busy rates, cache usage, queue, values processed, and a few other things. It is highly recommended to use these templates in all Zabbix installations.