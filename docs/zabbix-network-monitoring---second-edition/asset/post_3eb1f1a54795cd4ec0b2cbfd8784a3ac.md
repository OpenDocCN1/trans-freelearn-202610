
As can be seen, this machine has three interfaces: **loopback**, **Ethernet**, and a **tunnel**. The picture will be very different for some other devices. For example, an HP ProCurve switch would return (with the output shortened) the following:

```
$ snmpwalk -v 2c -c public 10.196.2.233 .iso.org.dod.internet.mgmt.mib-2.interfaces.ifTable.ifEntry.ifDescr
IF-MIB::ifDescr.1 = STRING: 1
IF-MIB::ifDescr.2 = STRING: 2
...
IF-MIB::ifDescr.49 = STRING: 49
IF-MIB::ifDescr.50 = STRING: 50
IF-MIB::ifDescr.63 = STRING: DEFAULT_VLAN
IF-MIB::ifDescr.4158 = STRING: HP ProCurve Switch software loopback interface
```

Now that we know the OID to use for dynamic index items, let's create one such item in Zabbix. Navigate to **Configuration | Hosts**, click on **Items** next to the correct host you want to create the item for, and click on **Create item**. Fill in the following values:

- **Name:** Outgoing traffic on interface $1
- **Type:** SNMPv2 agent
- **Key:** ifOutOctets[eth0]
- **SNMP OID:** IF-MIB::ifOutOctets["index","ifDescr","eth0"]
- **Units:** Bps
- **Store value:** Delta (speed per second)

Same as before, replace eth0 with an interface name that exists on the target system. When you are done, click on the **Add** button at the bottom.

> Make sure that the compound OID is entered correctly, paying close attention to quotes and spelling. We discussed the reason to use the **Numeric (unsigned)** type of information in *Chapter 3, Monitoring with Zabbix Agents and Basic Protocols.*

![](img/f5313a804532fabacc8d07cc8607eeba_160_0.png)

The newly added item should start gathering data, so let's look at **Monitoring | Latest data**. If you don't see this item or the data for it, navigate back to **Configuration | Hosts** and click on **Items** next to the corresponding host—there should be an error message displayed that should help with fixing the issue. If you have correctly added the item, you'll see the traffic data, as follows:

| LAST CHECK | LAST VALUE | CHANGE |
|---|---|---|
| 2016-03-18 19:40:07 | 37 Bps | -2 Bps |

> Remember that the index matches the exact string—a substring match will not work here.

Dynamic index items are quite common. Many network devices have fixed port names but varying indexes. Host-based SNMP agents place things such as disk usage and memory statistics in dynamic indexes; thus, if you have such devices to monitor, Zabbix support for them will be handy.

Using dynamic index items can slightly increase overall load, as two SNMP values are required to obtain the final data. Zabbix caches retrieved index information, so the load increase should not be noticeable.

A dynamic SNMP index enables us to easily monitor a specific interface or other entity by name, but it would not be a very efficient method for monitoring a larger number of interfaces. We will discuss an automated solution, low-level discovery, in *Chapter 11, Advanced Item Monitoring*.

## SNMP bulk requests

You might have spotted the checkbox next to the **SNMP interfaces** section, **Use bulk requests**:

![](img/f5313a804532fabacc8d07cc8607eeba_161_0.png)

Now, on to instructing snmptrapd to use that script. We only need to tell the trap daemon to process all the received traps with this script. To do this, you'll have to find the location where your distribution places the Net-SNMP configuration files—usually, /etc/snmp/. In this directory, look for a file named snmptrapd.conf. If it's there, edit it (create a backup copy before you do anything); if it's missing, create it. Edit it as root and make it look as follows:

```
authCommunity execute public
perl do "/home/zabbix/bin/zabbix_trap_receiver.pl";
```

This will accept all traps that have the community set to public and pass them to the Perl receiver script.

> If you expect to receive traps with various community strings that are not known in advance, you could disable the authorization or checking of the community string with the disableAuthorization yes option in snmptrapd.conf.

Start or restart the trap daemon. It might be worth taking a quick look at the zabbix_trap_receiver.pl file. Notice the line that specifies the path:

```
$SNMPTrapperFile = '/tmp/zabbix_traps.tmp';
```

Behind the scenes, traps are passed to the Zabbix server through a temporary file. We'll discuss this in a bit more detail later in this chapter.

## Filtering values by received data

Now on to the items on the Zabbix side. To test the most simple thing first, we will try to send values from the Zabbix server. Navigate to Configuration | Hosts, click on A test host in the NAME column, and click on Add in the SNMP interfaces section. Click on the Update button at the bottom, and then click on Items next to A test host. Click on Create item and enter these values:

- **Name:** SNMP trap tests
- **Type:** SNMP trap
- **Key:** snmptrap[test]
- **Type of information:** Character

When you're done, it should look like this:

![](img/f5313a804532fabacc8d07cc8607eeba_165_0.png)

This item will collect all traps that this host gets, if the traps contain the string test. We have the trap daemon configured to place traps in a file, and we have the item to place these traps in. What's left is telling the Zabbix server where to get the traps. Open zabbix_server.conf and modify the StartSNMPTrapper parameter:

```
StartSNMPTrapper=1
```

There is a special process in Zabbix that reads traps from a temporary file. This process is not started by default, so we changed that part of the configuration. Take a look at the parameter just above this one:

```
SNMPTrapperFile=/tmp/zabbix_traps.tmp
```

Notice how it matches the file in the Perl script. A change in the script should be matched by a change in this configuration file and vice versa. At this time, we will not change the location of this temporary file.

After these changes have been made, restart the Zabbix server daemon.

Now, we are ready to test this item. Let's send a trap by executing the following from the Zabbix server:

```
$ snmptrap -Ci -v 2c -c public localhost "" "NET-SNMP-MIB::netSnmpExperimental" NET-SNMP-MIB::netSnmpExperimental s "test"
```

This slightly non-optimal Net-SNMP syntax will attempt to send an SNMP trap to localhost using the public community and some nonsense OID. It will also wait for a response to verify that snmptrapd has received the trap successfully — this is achieved by the -Ci flag. It uses the default port, 162, so make sure the port is open in your firewall configuration on the Zabbix server to receive traps.

> Waiting for confirmation also makes snmptrap retransmit the trap. If the receiving host is slow to respond, the trap might be received multiple times before the sender receives confirmation.

If the command is successful, it will finish without any output. If it fails with the snmpinform: Timeout error message, then several things could have gone wrong. As well as double-checking that UDP port 162 is open for incoming data, verify that the community in the /etc/snmp/snmptrapd.conf file matches the one used in the snmptrap command and that the snmptrapd daemon is actually running.

If everything goes well, we should be able to see this item with a value on the latest data page:

| SNMP trap tests | 2016-03-18 23:17:48 | 23:17:46 2016/03/18 PDU INF... |

Now, let's send a different trap. Still on the Zabbix server, run this:

```
$ snmptrap -Ci -v 2c -c public localhost "" "NET-SNMP-MIB::netSnmpExperimental" NET-SNMP-MIB::netSnmpExperimental s "some other trap"
```

This trap will not appear in the item we created. What happened to it? As the value that we sent did not contain the string test, this value did not match the one in the item. By default, such traps are logged in the server logfile. If we check the logfile, it should have something similar to the following:

```
9872:20160318:232004.319 unmatched trap received from "127.0.0.1":
23:20:02 2016/03/18 PDU INFO:
  requestid          253195749
  messageid          0
  transactionid      5
  version            1
  notificationtype   INFORM
  community          public
  receivedfrom       UDP: [127.0.0.1]:54031->[127.0.0.1]:162
  errorindex         0
  errorstatus        0
VARBINDS:
  DISMAN-EVENT-MIB::sysUpTimeInstance type=67 value=Timeticks: (2725311) 7:34:13.11
  SNMPv2-MIB::snmpTrapOID.0          type=6  value=OID: NET-SNMP-MIB::netSnmpExperimental
  NET-SNMP-MIB::netSnmpExperimental type=4  value=STRING: "some other trap"
```
