+   - **SNMP OID**: This is where our knowledge comes in. Paste the SNMP OID you have found out and chosen here. In the example, `PowerNet-MIB::upsAdvIdentSerialNumber.0` is entered. If you are using the Net-SNMP daemon, enter `SNMPv2-MIB::sysDescr.0`
+   - **SNMP community**: Unless you have changed it, keep the default `public` value.
+   - **Type of information**: Select **Character**.
+   - **Update interval (in sec)**: This information doesn't really change that often, so use some large value, such as 86400.

![](bbox=[0.2, 0.292, 0.8, 0.548])

> If you left the agent interface in place, notice how it cannot be chosen for this item — only the SNMP interface can. While some item types can be assigned to any interface type, SNMP items must be assigned to SNMP interfaces.

When you are done, click on the Add button at the bottom.

Now, the outcome will depend on several factors. If you are lucky, you will already see the incoming data in **Monitoring | Latest data**. If you have chosen some vendor-specific OID, like in our example, it is possible that you will have to go back to **Configuration | Hosts**, click on **Items** next to **SNMP device**, and observe the status of this item:

![](bbox=[0.18, 0.82, 0.82, 0.915])