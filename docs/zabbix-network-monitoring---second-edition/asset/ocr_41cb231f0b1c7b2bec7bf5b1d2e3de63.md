Zabbix didn't like that, though. The data we provided was rejected because of the data type mismatch, thus it is clear that any process that is passing the data is responsible for the data contents and formatting.

Now, security-concerned people would probably ask — who can send data to items of the trapper type? A zabbix_sender can be run on any host by anybody, and it is enough to know the hostname and item key. It is possible to restrict this in a couple of ways — for one of them, see Configuration | Hosts, click on Items next to A test host and click on Amount of persons in the room in the NAME column. Look at one of the last few properties, Allowed hosts. We can specify an IP address or DNS name here, and any data for this item will be allowed from the specified host only:

![](bbox=[0.18, 0.42, 0.78, 0.48])

Several addresses can be supplied by separating them with commas. In this field, user macros are supported as well. We discussed user macros in Chapter 8, Simplifying Complex Configuration with Templates.

Another option to restrict who can send the data to trapper items is by using the authentication feature with PSK or SSL certificates. That is discussed in Chapter 20, Encrypting Zabbix Traffic.

# Using an agent daemon configuration file

So far, we specified all the information that zabbix_sender needs on the command line. It is also possible to automatically retrieve some of that information from the agent daemon configuration file. Let's try this (use the correct path to your agent daemon configuration file):

```
$ zabbix_sender -c /usr/local/etc/zabbix_agentd.conf -k room.persons -o 3
```

This succeeds, because we specified the configuration file instead of the Zabbix server address and the hostname — these were picked up from the configuration file. If you are running zabbix_sender on many hosts where the Zabbix agent also resides, this should be easier and safer than parsing the configuration file manually. We could also use a special configuration file for zabbix_sender that only contains the parameters it needs.

> If the ServerActive parameter contains several entries, values are sent only to the first one. The HostnameItem parameter is not supported by zabbix_sender.