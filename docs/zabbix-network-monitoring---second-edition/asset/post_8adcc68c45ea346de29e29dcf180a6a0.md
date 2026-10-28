
- While **Another host** does not have items of SNMP, IPMI, and other types, those could have started to fail because appropriate support was not compiled into the proxy, or respective pollers were not started
- If you configured the proxy on the Zabbix server system, passive items will work, as the IP address the agent gets the connections from will stay the same

Let's fix at least the passive agent items. Edit `zabbix_agentd.conf` on **Another host** and change the Server parameter. Either replace the IP address in there with the proxy address, or add the proxy address to it, then restart the agent. In a few minutes, most of the passive agent items should start receiving data again.

As for the ICMP items, refer to *Chapter 3, Monitoring with Zabbix Agents and Basic Protocols* for `fping` configuration. It's the same as on the server side; it's just that the changes have to be performed on the proxy system now.

In general, when a host is monitored by proxy, all connections to and from that host must and will be performed by the proxy. The agent must allow connections from the proxy for passive items and connect to the proxy for active items. Even the Zabbix sender must send data to the proxy for Zabbix trapper items, not the Zabbix server anymore.

With the host monitored by the proxy, let's check whether there is any indication of that in the frontend. Open **Configuration | Hosts**, make sure **Linux servers** is selected in the **Group** dropdown, and take a look at the **NAME** column. As can be seen, **Another host** is now prefixed by the proxy name and reads **First proxy:Another host**:

| NAME |
|---|
| First proxy: Another host |
| A test host |
| IPMI host |

![](img/f5313a804532fabacc8d07cc8607eeba_605_0.png)

> When having multiple proxies, it is a common practice to name them by location name—for example, proxy-London or Paris-proxy.

But do we always have to go to **Administration | Proxies** whenever we want to have a host monitored by proxy? Click on **Another host** in the **NAME** column, and observe the available properties. There's a dropdown available, **Monitored by proxy**. Using this dropdown, we can easily assign a host to be monitored by the chosen proxy (remembering to change the server IP address in the agent daemon configuration file):

![](img/f5313a804532fabacc8d07cc8607eeba_606_0.png)

> If you decide to monitor **A test host** through the proxy, be very careful with its address. If the address is left at `127.0.0.1`, the proxy will connect to the local agent for passive items and then report that data to the server, claiming it came from **A test host**. That would also be not that easy to spot, as the data would come in just fine; only it would be wrong data.

## Proxy benefits

With our first proxy configured, let's discuss in more detail its operation and the benefits it provides. Let's start with the main benefits:

- A proxy collects data when the server is not available
- A proxy reduces the number of connections to and from remote environments
- A proxy allows us to use incoming connections for polled items

We talked about the proxy retrieving configuration data from the server, and we talked about it having a local database. The Zabbix proxy always needs a local database, and this database holds information on the hosts the proxy is supposed to monitor. The same database also holds all the data the proxy has collected, and if the server cannot be reached, that data is not lost. For how long? By default, data is kept for one hour. This can be configured in the `zabbix_proxy.conf` file, the `ProxyOfflineBuffer` parameter. It can be set up to 30 days, but beware of running out of disk space, as well as of the potential to overload the Zabbix server when connectivity is back—we will discuss that risk in more detail later:

![](img/f5313a804532fabacc8d07cc8607eeba_607_0.png)

> There are more proxy-specific configuration parameters available; they are listed later in this chapter.

Fewer connections to remote environments can be very important, too. Monitoring using passive items means one connection for each value. With active items it's a bit better; multiple values will be sent in a single connection often. But the proxy pools up to 1000 values in a single connection. That is done even when they are of different types, such as agent, SNMP, IPMI, and SSH items. Fewer connections means healthier firewalls and other network devices, and much better performance from smaller total latency and less work for the Zabbix server to handle the incoming connections:

![](img/f5313a804532fabacc8d07cc8607eeba_607_1.png)

The third main benefit is the ability to receive incoming connections on the server side and still gather data by polling devices. For example, when monitoring a customer environment, the Zabbix server might have no access to the network devices. The Zabbix proxy could connect to them, collect data using SNMP, and then connect to the server to send the data. Also, keep in mind that only a single port for a single address would have to be opened in firewalls, as opposed to a lot of ports for all of the monitored devices when a proxy is not used:

![](img/f5313a804532fabacc8d07cc8607eeba_607_2.png)

There are a few more benefits Zabbix proxies provide:

- Single point of control for all proxies on the Zabbix server
- Ability to use multiple Java gateways

By default, proxies synchronize the configuration once per hour, and this period can be set in the zabbix_proxy.conf configuration file. Look for the parameter named ConfigFrequency, which by default will look like this:

```
# ConfigFrequency=3600
```

This means that a Zabbix proxy can lag in configuration up to an hour, which might sound scary, but once a production installation settles, the configuration usually doesn't change that often. While testing, you might wish to decrease this period, but in a stable production setup it is actually suggested to increase this value.

> If you must have configuration changes pushed to a proxy immediately, force the configuration to be reloaded.

The collected data is sent to the server every second by default. That can be customized in the zabbix_proxy.conf file with the DataSenderFrequency parameter.

> The active proxy won't connect to the server every second if it has no values to send—a 1-second interval will be used only if it has data to send. On the other hand, if it has lots of values to send and cannot push them all in a single connection (which means 1000 values), the next connection will be performed as soon as possible without waiting that one second.

Regarding host and host group maintenance, when a host is in maintenance without data collection, data is still sent by proxy, but the server discards it. This way, changes in the maintenance status do not suffer from the default one-hour delay for configuration sync.

## Proxies and availability monitoring

With all the benefits that a proxy brings, one would be tempted to use them a lot—and a good idea that would be, too. Proxies are really great. There's still the issue of monitoring availability for hosts behind proxies. If a proxy goes down or cannot communicate with the Zabbix server, we would be missing data for all the hosts behind that proxy. If we used the nodata() trigger function to detect unavailable hosts (we could call such triggers availability triggers), that could mean thousands of hosts declared unavailable. Not a desirable situation. There is no built-in dependency for hosts behind a proxy, but we can monitor proxy availability and set trigger dependencies for all hosts behind that proxy. But what should we set those dependencies to? Let's discuss the available ways to monitor proxy availability and their potential shortcomings.

## Method 1 – Last access item

There was the last access column in Administration | Proxies. Of course, looking at it all the time is not feasible, thus it can also be added as an internal item. To create such an item, let's go to Configuration | Hosts, click on Items next to the host that runs your proxy, and click on Create item. Fill in the following values:

- Name: $2: last access
- Type: Zabbix internal
- Key: zabbix[proxy,First proxy,lastaccess]
- Units: unixtime

> This item can be created on any host, but it is common to create it either on the Zabbix proxy host, or on the Zabbix server host.

In the key here, the second parameter is the proxy name. Thus, if your proxy was named kermit, the key would become zabbix[proxy,kermit,lastaccess].

> If items like these are created on hosts that represent the proxy system and have the same name as the proxy, a template could use the {HOST.HOST} macro as the second parameter in this item key. We discussed templates in Chapter 8, Simplifying Complex Configuration with Templates.

When done, click on the Add button at the bottom. Notice how we used a special unit here — unixtime. Now what would it do? To find out, navigate to Monitoring | Latest data, expand the Filter, select the host you created the last item on and enter proxy in the Name field, then click on the Filter button. Look at the way data is presented here — we can see very nicely, in a human-readable form, when the proxy last contacted the Zabbix server:

| NAME ▲ | LAST CHECK | LAST VALUE |
|---|---|---|
| - other - (1 item) | | |
| First proxy: last access | 2015-11-24 17:14:59 | 2015-11-20 17:27:53 |
