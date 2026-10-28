In case this trap was not sorted properly and still went into the **snmptraps** host, it could be caused by different output in some Net-SNMP versions. Instead of passing the IP address or hostname of the incoming connection as the first value, they pass a string like this:

```
UDP: [192.168.56.11]:56417->[192.168.56.10]:162
```

In that case, try adding another line before the `zabbixhost` assignment:

```
oid=`echo $oid|cut -f11 -d'.'`
community=`echo $community|cut -f2 -d'"'`
hostname=$(echo "$hostname" | awk -F'[][]' '{print $2}')
```

It will extract the first string enclosed in square brackets from the `hostname` variable. After making this change to the script, send the trap again.

That took us some time to set up, but now it's very simple. If we want traps from some host to be handled by a specific host, we create that host and an `snmptraps` item for it. All other traps go to the generic **snmptraps** host and **snmptraps** item.

But what about item lookup? The database holds information on item keys as well, so perhaps we could try using that.

We need to retrieve the item key from any database field based on the information received in the trap. As traps include SNMP OIDs, they are the best candidates to map traps against items. Now, the OID can be in numeric or textual form. In the Zabbix configuration, we have two fields that could be used:

+   - **Name**: While pretty much a freeform field, it is a "friendly name," so we'd better keep it human-readable.
+   - **Key**: This field has more strict rules on the characters it accepts, but OIDs should be fine. While not used by humans much, this field is still referred to in the trigger expressions.

That means we will use the **Key** field. To keep it both short enough and somewhat human-readable, we'll set it to the last part of the received textual-form OID. As the trap will be received by `snmptraps.sh`, it will try to match the received OID to the item key and based on that decide where to send the data.

> Remember that specific MIBs might have to be added to `/etc/snmp/snmp.conf` so that they are found by `snmptrapd`.