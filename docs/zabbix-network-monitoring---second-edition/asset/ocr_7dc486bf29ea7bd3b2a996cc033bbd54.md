This approach also provides an easy way to monitor selected interfaces only. If you have a large number of network devices and only a few selected ports are to be monitored, the description for those ports could be changed on the device—for example, they could all be prefixed with zbx. This will show up in the ifAlias OID, and we would filter by the {#IFALIAS} macro in the LLD rule properties.

> The macro names are user configurable and could be different on a different Zabbix installation. Only the built-in {#SNMPINDEX} macro will always have the same name.

# Creating custom LLD rules

The built-in low-level discovery support is great for discovering filesystems, network interfaces, CPUs, and other entities. But what if we have some custom software that we would like to discover components with or perhaps are running an older Zabbix agent on some system that does not support a particular type of discovery yet? The great thing about LLD is that it is very easy to extend with our own discovery rules. Let's take a look at two examples:

+   - Re-implementing CPU discovery on Linux
+   - Discovering MySQL databases

> An LLD rule never returns item values. It discovers entities that allow creating items from prototypes. Items receive values from agents, SNMP devices, using zabbix_sender, or any of the other data-collection methods.

# Re-implementing CPU discovery

First, let's try to do something that is already available in recent Zabbix agents—discovering CPUs. We do this both because it could be useful if you have some system running an old agent and because it shows how simple LLD can be sometimes. To do this, let's consider the following script:

```
for cpu in $(ls -d /sys/devices/system/cpu/cpu[0-9]*/); do
  cpui=${cpu#/sys/devices/system/cpu/cpu}
  [[ $(cat ${cpu}/online 2>/dev/null) == 1 || ! -f ${cpu}/online ]] &&
  status=online || status=offline;
  cpulist=$cpulist,'{#CPU.NUMBER}: '${cpui$/' ',
  {#CPU.STATUS}: '$status'}'
done
echo '{data: ['${cpulist#, }']}'
```