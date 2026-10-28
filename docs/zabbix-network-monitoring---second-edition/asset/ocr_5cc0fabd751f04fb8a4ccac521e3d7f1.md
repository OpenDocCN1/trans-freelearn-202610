Now what's that? How could it be? We saw in our tests with Net-SNMP command line tools that there actually is such an OID. Well, one possible situation when this error message appears is when the specified MIB is not available, which could happen if you tried SNMP queries previously from a different host.

Zabbix server works as if ALL is set for MIB contents; thus, you don't have to do anything besides copying the MIB to the correct directory (usually `/usr/share/snmp/mibs`) on the Zabbix server and restarting the server daemon. If you did not copy the OID, instead deciding to retype it, you might have made a mistake. Verify that the entered OID is correct.

> The error message in the Zabbix frontend might be misleading in some cases. Check the server log to be sure.

After fixing any problems, wait until Zabbix server refreshes the item configuration and rechecks the item. With the item configured, let's see what data we can get in Zabbix from it. Navigate to **Monitoring | Latest data**, expand the filter, clear the **Host groups** field, and start typing SNMP in the **Host** field — **SNMP device** should appear, so choose it and click on **Filter**. Expand the other category if needed, and look for the serial number. You should see something like this:

![](bbox=[0.234, 0.494, 0.767, 0.527])

The serial number has been successfully retrieved and is visible in the item listing. This allows us to automatically retrieve data that, while not directly tied to actual availability or performance monitoring, is still quite useful. For example, if a remote device dies and has to be replaced, you can easily find the serial number to supply in a servicing request, even if you neglected to write it down beforehand.

# Translating SNMP OIDs

In case you can't or don't want to copy vendor-specific MIB files to the Zabbix server, you can always use numeric OIDs, like we did before. While not being as descriptive, they are guaranteed to work even if the copied MIBs are not available for some reason or are removed during a system upgrade.