Listed here are the default ports that can be changed if necessary:

+   - Normal or passive items: The Zabbix server connects to a Zabbix agent, which in turn gathers the data
+   - Active items: The Zabbix agent connects to a Zabbix server, retrieves a list of things to monitor, gathers the data, and then periodically reports back to the server
+   - Simple checks: The Zabbix server directly queries the exposed network interfaces of the monitored host; no agent is required

The simple checks were different: they never used the Zabbix agent and were performed directly from the Zabbix server. Simple checks included TCP port checking.

This covers the two basic, most commonly used check types: a Zabbix agent with bidirectional connection support and simple checks that are performed directly from the server.

In the next chapter, we will look at SNMP monitoring. We'll start with a quick introduction to the Net-SNMP tools and basic Management Information Base (MIB) management and will set up SNMP polling with fixed and dynamic OIDs. We will also receive SNMP traps and map them to hosts and items both using the built-in method and a very custom approach.