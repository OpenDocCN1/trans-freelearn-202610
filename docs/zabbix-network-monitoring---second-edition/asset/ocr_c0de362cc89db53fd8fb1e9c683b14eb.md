Host name SNMP device

Visible name

Groups In groups Other groups

SNMP devices

Database servers
Discovered hosts
Hypervisors
Linux servers
Network devices
Templates
UPS devices
Virtual machines
Web servers
Windows servers

New group

Agent interfaces IP ADDRESS DNS NAME CONNECT TO PORT DEFAULT
127.0.0.1 IP DNS 10050 Remove
Add

SNMP interfaces
192.168.56.11 IP DNS 161 Remove
Use bulk requests
Add

> If no agent items will be created for this host, the agent interface will be ignored. You may keep it or remove it.

When you are done, click on the Add button at the bottom. It's likely that you won't see the newly created host in the host list. The reason is the Group dropdown in the upper-right corner, which probably says Linux servers. You can change the selection to All to see all configured hosts or to SNMP devices to only see our new device. Now is the time to create an item, so click on Items next to SNMP devices and click on the Create item button. Fill in the following values:

+   - Name: Enter something sensible, such as Serial number, if you are using an OID from an SNMP agent, or System description if you are using the Net-SNMP daemon.
+   - Type: Change to the appropriate version of your SNMP agent. In the displayed example, SNMPv1 agent is chosen because that's the only version our device supports.
+   - Key: This is not restricted or too important for SNMP items, but required for references from triggers and other locations. You can choose to enter the last part of the textual OID, such as upsAdvIdentSerialNumber.0 or sysDescr.0.