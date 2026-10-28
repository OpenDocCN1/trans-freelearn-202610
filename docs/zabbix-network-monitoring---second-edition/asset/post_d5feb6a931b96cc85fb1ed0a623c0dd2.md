
The agent is trying to request the active check list, but the connection fails. The attempt seems to be wrong – our Zabbix server should be on a different system than the localhost. Let's see how we can fix this. On the remote machine, open the zabbix_agentd.conf configuration file and check the ServerActive parameter. The default configuration file will have a line like this:

```
ServerActive=127.0.0.1
```

This parameter tells the agent where it should connect to for active items. In our case, the localhost will not work as the Zabbix server is on a remote machine, so we should modify this. Replace 127.0.0.1 with the IP address or DNS name of the Zabbix server, and then restart the agent either using an init script or the manual method killall.

While you have the configuration file open, take a look at another parameter there—StartAgents. This parameter controls how many processes are handling incoming connections for passive items. If set to 0, it will prevent the agent from listening on incoming connections from the server. This enables you to customize agents to support either or both of the methods. Disabling passive items can be better from a security perspective, but they are very handy for testing and debugging various problems. Active items can be disabled by not specifying (commenting out) ServerActive. Disabling both active and passive items won't work; the agent daemon will complain and refuse to start up, and it's correct—starting with both disabled would be a pointless thing to do. Take a look:

```
zabbix-agentd [16208]: ERROR: either active or passive checks must be enabled
```

We could wait for values to appear on the frontend again, but again, they would not. Let's return to the agent daemon logfile and see whether there is any hint about what's wrong:

```
15938:20141230:154544.559 no active checks on server [10.1.1.100:10051]: host [Zabbix server] not monitored
```

If we carefully read the entry, we will notice that the agent is reporting its hostname as Zabbix server, but that is the hostname of the default host, which we decided not to use and left disabled. The log message agrees: it says that the host is not monitored.

If we look at the startup messages, there's even another line mentioning this:

```
15931:20141230:154544.552 Starting Zabbix Agent [Zabbix server]. Zabbix 3.0.0 (revision 58567).
```

> You might or might not see the SVN revision in this message depending on how the agent was compiled. If it's missing, don't worry about it as it does not affect the ability of the agent to operate.

As that is not the host name we want to use, let's check the agent daemon configuration file again. There's a parameter named Hostname, which currently reads Zabbix server. Given that the comment for this parameter says "Required for active checks and must match hostname as configured on the server.", it has to be what we're after. Change it to Another host, save and close the configuration file, and then restart the Zabbix agent daemon. Check for new entries in the zabbix_agentd.log file — there should be no more errors.

While we're at it, let's update the agent configuration on A test host as well. Modify zabbix_agentd.conf and set Hostname=A test host and restart the agent daemon.

If there still are errors about the host not being found on the server, double-check that the hostname in the Zabbix frontend host properties and agent daemon configuration file (the one we just changed) match.

> This hostname is case sensitive.

It's now time to return to the frontend and see whether data has started flowing in at the Monitoring | Latest data section:

| HOST | NAME ▲ | LAST CHECK | LAST VALUE | CHANGE |
|---|---|---|---|---|
| Another host | - other - (3 Items) |  |  |  |
|  | Incoming traffic on interface enp0s8 | 2016-02-20 01:45:20 | 173013 | +1296 |

> Notice how the system in this screenshot actually has an interface named enp0s8, not eth0. We will find out how to allow Zabbix to worry about interface names and discover them automatically in Chapter 12, Automating Configuration.

If you see no data and the item shows up unsupported in the configuration section, check the network interface name.

Great, data is indeed flowing, but the values look really weird. If you wait for a while, you'll see how the number in the LAST VALUE column just keeps on increasing. So what is it? Well, network traffic keys gather data from interface counters, that is, the network interface adds up all traffic, and this total data is fed into the Zabbix database. This has one great advantage: even when data is polled at large intervals, traffic spikes will not go unnoticed as the counter data is present, but it also makes data pretty much unreadable for us, and graphs would also look like an ever-growing line (if you feel like it, click on the Graph link for this item). We could even call them "hill graphs". Luckily, Zabbix provides a built-in capability to deal with data counters like this. Go to Configuration | Hosts, then click on Items next to Another host, and click on Incoming traffic on interface eth0 in the NAME column. Change the Store value dropdown to read Delta (speed per second), and then click on Update.

We will have to wait a bit for the changes to take effect, so now is a good moment to discuss our choice for the Type of information option for this item. We set it to Numeric (unsigned), which accepts integers. The values that this item originally receives are indeed integers – they are counter values denoting how many bytes have been received on this interface. The Store value option we changed to Delta (speed per second), though, will almost always result in some decimal part being there – it is dividing the traffic between two values according to the number of seconds passed between them. In cases where Zabbix has a decimal number and has to store it in an integer field, the behavior will differ depending on how it got that decimal value, as follows:

+   - If the decimal value arrived from a Zabbix agent source like a system.cpu.load item, the item will turn up unsupported
+   - If Zabbix received an integer but further calculations resulted in a decimal number appearing, like with our network item, the decimal part will be discarded

This behavior is depicted in the following figure:

![](img/f5313a804532fabacc8d07cc8607eeba_111_0.png)

Why is there a difference like this, and why did we leave this item as an integer if doing so results in a loss of precision? Decimal values in the Zabbix database schema have a smaller number of significant digits available before the decimal point than integer values. On a loaded high-speed interface, we might overflow that limit, and it would result in values being lost completely. It is usually better to lose a tiny bit of precision—the decimal part—than the whole value. Note that precision is lost on the smallest unit: a byte or bit. Even if Zabbix shows 5 Gbps in the frontend, the decimal part will be truncated from this value in bits; thus, this loss of precision should be really, really insignificant. It is suggested to use integers for items that have a risk like this, at least until database schema limits are increased.

Check out **Monitoring | Latest data** again:

| HOST | NAME ▲ | LAST CHECK | LAST VALUE | CHANGE |
|---|---|---|---|---|
| Another host | - other - (3 Items) |  |  |  |
|  | Incoming traffic on interface enp0s8 | 2016-02-20 01:50:20 | 29 | -177700 |

> Keep in mind that in the worst case, configuration changes might take up to 3 minutes to propagate to the Zabbix agent: 1 minute to get into the server configuration cache and 2 minutes until the agent refreshes its own item list. On top of this delay, this item is different from the others we created: it needs to gather two values to compute per second, one of which we are interested in; thus, we will also have to wait for whatever the item interval is before the first value appears in the frontend.

That's better; Zabbix now automatically calculates the change between every two checks (that's what the delta is for) and stores it, but the values still don't seem to be too user friendly. Maybe they're better in the graph—let's click on the **Graph** link to find out:

![](img/f5313a804532fabacc8d07cc8607eeba_112_0.png)
