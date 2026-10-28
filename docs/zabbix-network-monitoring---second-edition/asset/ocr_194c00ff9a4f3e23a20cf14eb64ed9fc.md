# SNMP devices

+   - Returning unclean data can also easily break data retrieval. When retrieving data with user parameters, make sure it does not contain characters making it unsuitable for storage (such as returning `26.6 C` for a float datatype item) or has other weird characters (such as having a CR/LF newline at the end of the data string).
+   - By default, agent items will time out after three seconds. It is not suggested to increase this timeout in most cases, although it might be reasonably safe to do so if the userparameter variable is used as an active item. Remember that active items are not parallel — only one agent process works on them, one item at a time. Consider using `zabbix_sender` for such items instead.

+   - **Q:** My SNMP items do not work.
+   - **A:** Double-check that the SNMP version and community string are set correctly. Specifying an incorrect SNMP version will often cause timeouts, making it harder to debug. Of course, check general connectivity and permissions by using the `snmpwalk` and `snmpget` commands from the Zabbix server.
+   - **Q:** My SNMP items either do not work at all or fail frequently.
+   - **A:** Perhaps your device does not properly support `SNMP GETBULK`—try disabling bulk get support in the host properties for the SNMP interface.
+   - **Q:** I imported a template, but the LLD fails with `Invalid SNMP OID: pairs of macro and OID are expected.`
+   - **A:** The Zabbix SNMP LLD key syntax changed in Zabbix 2.4. Unfortunately, the XML import process was not updated accordingly, and the imported LLD rule uses the old syntax. Refer to *Chapter 12, Automating Configuration*, for details on the key syntax.
+   - **Q:** I added MIB files, and they work with the command line tools, but Zabbix Server seems to be ignoring the MIB files.
+   - **A:** Make sure to restart the server daemon—MIB definitions are loaded only upon startup.

> Additionally, make sure you are not overloading the monitored device by querying lots of values too frequently.