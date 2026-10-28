
## SNMP devices

- Returning unclean data can also easily break data retrieval. When retrieving data with user parameters, make sure it does not contain characters making it unsuitable for storage (such as returning `26.6 C` for a float datatype item) or has other weird characters (such as having a CR/LF newline at the end of the data string).
- By default, agent items will time out after three seconds. It is not suggested to increase this timeout in most cases, although it might be reasonably safe to do so if the userparameter variable is used as an active item. Remember that active items are not parallel — only one agent process works on them, one item at a time. Consider using `zabbix_sender` for such items instead.

- **Q:** My SNMP items do not work.
- **A:** Double-check that the SNMP version and community string are set correctly. Specifying an incorrect SNMP version will often cause timeouts, making it harder to debug. Of course, check general connectivity and permissions by using the `snmpwalk` and `snmpget` commands from the Zabbix server.
- **Q:** My SNMP items either do not work at all or fail frequently.
- **A:** Perhaps your device does not properly support `SNMP GETBULK`—try disabling bulk get support in the host properties for the SNMP interface.
- **Q:** I imported a template, but the LLD fails with `Invalid SNMP OID: pairs of macro and OID are expected.`
- **A:** The Zabbix SNMP LLD key syntax changed in Zabbix 2.4. Unfortunately, the XML import process was not updated accordingly, and the imported LLD rule uses the old syntax. Refer to *Chapter 12, Automating Configuration*, for details on the key syntax.
- **Q:** I added MIB files, and they work with the command line tools, but Zabbix Server seems to be ignoring the MIB files.
- **A:** Make sure to restart the server daemon—MIB definitions are loaded only upon startup.

> Additionally, make sure you are not overloading the monitored device by querying lots of values too frequently.

- **Q:** My SNMP items work, but some OIDs on a specific device do not, even though data appears in `snmpwalk` output.
- **A:** Try `snmpget` with those OIDs. Some UPSes are known to have buggy firmware that prevents these metrics from working with `GET` requests, but they do work with the `GETNEXT` requests that `snmpwalk` uses. If this is the case, upgrade the firmware on the device.
- **Q:** I listed all SNMP MIBs I want to use in `/etc/snmp/snmp.conf`, but Net-SNMP utilities do not use them all properly.
- **A:** Some Net-SNMP versions silently trim lines in this file to 1024 symbols, including the newline character. Try splitting options on multiple lines so that a single line does not exceed 1023 printable characters.
- **Q:** I'm monitoring network traffic, but it returns incorrect data.
- **A:** If it's a high-speed interface; make sure to use 64-bit counter OIDs such as `ifHCInOctets` and `ifHCOutOctets`.
- **Q:** I'm adding SNMP devices to Zabbix, but adding new devices stops the monitoring of the previous devices. If I query each device with `snmpget`, they still respond as expected.
- **A:** If it's SNMPv3, make sure all devices have a unique `snmpEngineID` variable.

## IPMI monitoring

- **Q:** I can't get the IPMI item to work.
- **A:** There are several things to verify when IPMI items do not work:
  - Make sure that the Zabbix server is configured with IPMI support. Simple, but easy to miss.
  - Check whether the `StartIPMIPollers` option in the server's configuration file is set to the default value, `0`. If it is, set it to `1` and restart Zabbix Server.
  - Make sure that the sensor names are correct. You can get the sensor names with `IPMItool`, and you have to use the name as it appears in the `IPMItool` output, with spaces and without quoting it.
  - Check using the latest OpenIPMI version. Older OpenIPMI versions are known to have various issues.

## ICMP checks

- **Q:** All of my ICMP checks are failing.
- **A:** Here are a few possible reasons:

  Check that `fping` may be run `setuid` as root by the user runs on Zabbix Server.

  Make sure SELinux does not prevent Zabbix from running `fping`. The `grep fping /var/log/audit/audit.log` command might reveal more information.

## Problems with simple checks

- **Q:** My service works, but the `net.tcp.service` or `net.udp.service` item says it does not.
- **A:** Besides verifying that the correct server is queried, check whether the service responds as Zabbix expects—simple checks are documented in detail at `https://www.zabbix.com/documentation/3.0/manual/appendix/items/service_check_details`.

## Problems with zabbix_sender and trapper items

- **Q:** I send in values with a timestamp, but a different timestamp is entered in the server database.
- **A:** The Zabbix sender includes the current time on the host in a `clock` property for the whole request, and Zabbix server adjusts the timestamp for all values accordingly. It is not possible to tell the server not to do so or the sender not to send it. Either fix the time on the sending system or implement the basic protocol without sending the request timestamp.

## General issues

- **Q:** I am monitoring network traffic, but the numbers are unrealistically huge.
- **A:** As the data is likely provided as a counter, make sure the result is stored as delta (speed per second) on Zabbix.
- **Q:** I'm monitoring a 10G interface speed in bytes per second, and when the interface is loaded, I lose values.
- **A:** Make sure **Type of information** is set to **Numeric (unsigned)**. This way, you'll lose the precision of a fraction of a bit, but keep all the values.
- **Q:** Zabbix does not like the formula for my calculated item.

- A: Make sure to use proper quoting, especially if the referenced item keys have quotes. For example, if the referenced item key is `key["parameter",param]`, in the calculated item formula, it can be used like this: `last("key[\"parameter\",param]")`. Notice the escaping of the inner double quotes with backslashes.
- Q: I'm trying to use an item key such as `proc.num['apache']`, but it does not work.
- A: Zabbix supports only double quotes; do not use single quotes for quoting.
- Q: I'm trying to use a trigger expression such as `{host:item.LAST()=13}`, but it does not work.
- A: Case sensitivity – almost everything is case-sensitive in Zabbix: item keys, their parameters, host names, trigger functions, and so on. If you come from Windows, keep reminding yourself that case matters.

## Triggers

- Q: My trigger does not work, or Zabbix refuses to add my trigger.
- A: Check the trigger's syntax, paying close attention to parentheses—is the correct type used? Are they all properly closed? The same goes for quotes, and remember about case sensitivity. Try splitting up complex expressions to pinpoint the error.

## Actions

- Q: My actions do not work.
- A: If the notifications do not appear in **Reports | Action log**, make sure the user you want to send notifications to has read permission to at least one of the hosts that participated in generating the event. Also check the action conditions, host maintenance settings, and action operations. Make sure your actions are not disabled—Zabbix can silently and automatically disable actions if the resources referenced in action conditions or operations are deleted. Also check the user media settings, such as severity and time filter, and whether the configured media type is enabled. If the messages do appear in the action log and there are error messages, hopefully the error is helpful. If the messages appear in the action log as successfully sent, check the logs on your MTA or other receiving system.
- Q: My e-mail notifications are not sent, and I can see error messages such as `[127.0.0.1] did not issue MAIL/EXPN/VRFY/ETRN during connection to MTA in e-mail server log files.`
