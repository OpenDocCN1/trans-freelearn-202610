# ## 19
# Using Proxies to Monitor Remote Locations

The Zabbix server can do monitoring using lots of different methods—it can communicate with Zabbix agents, SNMP devices, and IPMI devices; run commands; and a whole lot of other things. A problem arises when the number of devices to be monitored increases—a single endpoint (our Zabbix server) is supposed to communicate with lots of others, and a large number of connections can cause problems both on the Zabbix server and in the network components between the Zabbix server and monitored devices.

It gets worse if we have to monitor remote environments—be it a branch office, another data center, or a customer site. Zabbix agents? Port 10050 must be open to all servers. SNMP? Port 161 must be open to all devices. It becomes unmanageable real quick.

A solution is to use Zabbix proxies. A Zabbix proxy is a remote data collector process that is capable of collecting data using all the methods the Zabbix server supports. In this chapter, we will set up a Zabbix proxy, use it for data gathering, and discuss the best methods to determine whether the proxy itself is available.

> > Zabbix proxies are not available for Windows.