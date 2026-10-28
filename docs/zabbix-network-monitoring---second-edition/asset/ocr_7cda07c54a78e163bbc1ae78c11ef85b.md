+   - **StartPollers**: Pollers are internal processes that collect data in various ways. By default, five pollers are started, and this is plenty for tiny installations such as our test setup. In larger installations, it is common to have hundreds of pollers. Notice that there are no separate SNMP pollers—the same processes are responsible for passive agent and SNMP device polling. How to know whether you have enough? Using the internal monitoring, find out the average busy rate. If it's above 70%, just add more pollers. Pollers are responsible for:
+     - Connecting to passive agents
+     - Connecting to SNMP devices
+     - Performing simple checks, such as service/port checks
+     - Retrieving internal monitoring data
+     - Retrieving VMware data from the VMware cache
+     - Running external check scripts

+   - **StartIPMIPollers**: This specifies how many processes should be started that poll IPMI devices. We configured this parameter in *Chapter 16, Monitoring IPMI devices*.

+   - **StartPollersUnreachable**: If a host is not reachable, it is not polled by normal pollers anymore—special types called **unreachable pollers** now deal with that host, including IPMI items. This is done to avoid a situation where a few hosts that time out take up most of the poller time. If there aren't enough unreachable pollers, the worst thing that happens is that hosts, declared unreachable, are not noticed as being back up as quickly. By default, only one unreachable poller is started. To know whether that is enough, observe their busy rate, especially when there are systems down in the monitored environment.

+   - **StartTrappers**: By default, there are five trappers. As with pollers, monitor their busy rate and add more as needed. Trappers are responsible for receiving incoming connections from:
+     - Active agents
+     - Active proxies
+     - `zabbix_sender`
+     - The Zabbix frontend, including server availability check, global scripts, and queue data