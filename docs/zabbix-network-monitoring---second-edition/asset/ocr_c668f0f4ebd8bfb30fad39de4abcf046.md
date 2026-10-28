# Using Proxies to Monitor Remote Locations

In the following section, Hosts allows us to specify which hosts will be monitored by this proxy. To make one host monitored by the proxy, select Another host in the Other hosts list box and click on the ◄ button:

![](bbox=[0.075, 0.156, 0.925, 0.545])

When done, click on Add. Next time the proxy connects to the server, the names should match and the proxy should get the information on what it is supposed to monitor. But when will that next time be? By default, the Zabbix proxy connects to the Zabbix server once per hour. The first connection attempt happens upon proxy startup, and at one-hour intervals from then on. If you configured the frontend part soon after the proxy was started, it could take up to an hour for the proxy to get the configuration data and start working. There are two ways to force re-reading of the configuration data from the Zabbix server:

+   - Restart the proxy
+   - Force reloading of its configuration cache

The first one would be acceptable on our test proxy, but it would not be that nice on a larger production proxy that is actively collecting data already. Let's see how we can force reloading of the configuration cache. First run:

```
# zabbix_proxy --help
```