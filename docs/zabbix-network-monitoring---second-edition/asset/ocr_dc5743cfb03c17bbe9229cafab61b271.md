# Viewing the results

After a few minutes have passed, check the **Monitoring | Discovery** section:

| DISCOVERED DEVICE | MONITORED HOST | UPTIME/DOWNTIME | ICMP PING |
|---|---|---|---|
| **A test discovery** (2 devices) |  |  |  |
| 192.168.56.10 (testhost) |  | 00:23:22 | 🟩 |
| 192.168.56.11 | Another host | 00:23:20 | 🟩 |

All the devices that respond to the ping in the configured range will be listed here. If a device is already monitored as a host in Zabbix, it will be listed in the **MONITORED HOST** column. We will also see for how long the host is known to be up, and the **ICMP PING** column will list this specific service in green for all hosts. But why is only one host listed as already monitored here? Hosts are recognized here by their IP addresses, and for A test host, we used `127.0.0.1`. The address by which it was discovered differs, so it's not really considered to be the same host or device.

> Hosts are not clickable here at this time—probably the easiest way to get to the host properties is copying and pasting the hostname in the global search field.

Now, navigate back to **Configuration | Discovery** and click on **A test discovery** in the **NAME** column. Click on **New** in the **Checks** block and choose a service that is accessible and would be easy to control on these hosts—perhaps **SMTP** again. Click on **Add** in the **Checks** section, and then click on **New** there again. This time, choose a service that is not present on any host in the configured range—**FTP** might be a good choice. Then, click on **Add** in this block again:

![](bbox=[0.316, 0.716, 0.686, 0.804])