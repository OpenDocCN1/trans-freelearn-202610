> Getting SNMP MIBs isn't always easy. A certain large printer manufacturer representative claimed that they do not provide SNMP MIBs, and everybody should use their proprietary printer-management application. Most manufacturers do provide MIBs, though, and in some cases, freely accessible MIB collection sites can help better than official vendor sites.

After downloading a new MIB, you have to place it in a location where Net-SNMP will search for MIB files and configure them as well. Net-SNMP searches for MIBs in two locations: .snmp/mibs in the user's home directory and /usr/share/snmp/mibs; which one you use is your decision. If you want something for the current user only, or don't have access to the /usr directory, you can use .snmp/mibs; otherwise, use /usr/share/snmp/mibs. Whichever you choose, that's not enough—you also have to instruct tools to include this MIB.

> While Zabbix server uses the same directory to look for MIBs, specifying MIBs to be used is only required for the Net-SNMP tools—Zabbix server loads all MIBs found.

The first method is to pass MIB names directly to the called command. But hey, we don't know the MIB name yet. To find out what a particular name in some file is, open the file in a text editor and look for MIB DEFINITIONS ::= BEGIN near the beginning of the file. The string before this text will be the MIB name we are looking for. Here's an example:

```
PowerNet-MIB DEFINITIONS ::= BEGIN
```

So, APC has chosen to name its MIB PowerNet-MIB. Armed with this knowledge, we can instruct any command to include this file:

```
$ snmpget -m +PowerNet-MIB -v 1 -c public <IP address> SNMPv2-SMI::enterprises.318.1.1.1.1.2.3.0
PowerNet-MIB::upsAdvIdentSerialNumber.0 = STRING: "QS0547120198"
```

Excellent; snmpget included the correct MIB and obtained the full textual string, which confirms our suspicion that this might be a serial number. You can now use the same flag for snmpwalk and obtain a file with much better value names. Quite often, you will be able to search such a file for interesting strings such as serial number and find the correct OID.

> The + sign instructs us to include the specified MIBs in addition to otherwise configured ones. If you omit the +, the MIB list will be replaced with the one you specified.