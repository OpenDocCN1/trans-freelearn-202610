# Monitoring SNMP Devices

We've done with configuring Zabbix for SNMP trap receiving, but how will the traps get to the script we edited and, in turn, to Zabbix? The same as before, this is where `snmptrapd` steps in.

Let's create a simplistic configuration that will pass all the received traps to our script. To do this, we will edit `snmptrapd.conf`. If you created it earlier, edit it (you may comment out the lines we added previously); if it's missing, create the file. Edit it as root and make it look as follows:

```
authCommunity execute public
#perl do "/home/zabbix/bin/zabbix_trap_receiver.pl";
traphandle default /bin/bash /home/zabbix/bin/snmptrap.sh
```

We commented out the Perl receiver line and added a line to call our new script. The default keyword will make sure that all received traps go to this script (that is, unless we have other traphandle statements with OIDs specified, in which case only those received traps will get to this script that don't match any other traphandle statement). Save the file, and then start or restart the `snmptrapd` daemon as appropriate for your distribution.

Now, we should be able to receive SNMP traps through all the chain links. Let's test that by sending a trap same the way as before from the Zabbix server:

```
$ snmptrap -Ci -v 2c -c public localhost "" "NET-SNMP-MIB::netSnmpExperimental" NET-SNMP-MIB::netSnmpExperimental s "test"
```

Once the command completes successfully, check the frontend for the results. Go to **Monitoring | Latest data** and select **SNMP devices** in the filter:

![](bbox=[0.076, 0.597, 0.921, 0.627])

Great, data from our test trap has been received here. It's trimmed in the table view, though, so click on **History** to view all of it:

| TIMESTAMP | VALUE |
|---|---|
| 2016-03-19 00:41:52 | localhost "test" NET-SNMP-MIB::netSnmpExperimental |