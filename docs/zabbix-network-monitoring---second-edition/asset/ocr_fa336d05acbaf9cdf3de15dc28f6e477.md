# 11

# Advanced Item Monitoring

Having set up passive and active Zabbix agent items, simple checks such as ICMP ping or TCP service checks, or SNMP and IPMI checks, can we go further? Of course we can. Zabbix provides several more item types that are useful in different situations – let’s try them out.

In this chapter, we will explore log file monitoring, computing values on the server from the already collected data, running custom scripts on the Zabbix server or agents, sending in complete custom data using a wonderful utility, zabbix_sender, and running commands over SSH and Telnet. Among these methods, we should be able to implement monitoring of any custom data source that is not supported by Zabbix out of the box.

# Log file monitoring

Log files can be a valuable source of information. Zabbix provides a way to monitor log files using the Zabbix agent. For that, two special keys are provided:

+   - `log`: Allows us to monitor a single file
+   - `logrt`: Allows us to monitor multiple, rotated files

Both of the log monitoring item keys only work as active items. To see how this functions, let’s try out the Zabbix log file monitoring by actually monitoring some files.