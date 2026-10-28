This allows you to send a value to the Zabbix server, specifying the server with the `-z` flag, port with `-p`, and so on. Now let's return to the script. With our new knowledge, let's look at the last line—the one that invokes `zabbix_sender`. The script seems to pass values retrieved from the SNMP trap as parameters to `zabbix_sender`; thus, we can't make any decisions and information transformation between `snmptrapd` and Zabbix. Now, let's fix the problem we noticed:

+   - Change `ZABBIX_SENDER` to read `/usr/local/bin/zabbix_sender` (or another path if that's different for you)
+   - Additionally, change the last line to read `$ZABBIX_SENDER -z $ZABBIX_SERVER -p $ZABBIX_PORT -s "$HOST" -k "$KEY" -o "$str"`—this way, we are also quoting host and key names, just in case they might include spaces or other characters that might break command execution

Save the file. Let's prepare the Zabbix side now for trap receiving. On the frontend, navigate to **Configuration | Hosts** and click on **Create host**. Fill in the following values:

+   - **Name:** `snmptraps`
+   - **Groups:** Click on **SNMP devices** in the **Other groups** box, then click on the left-arrow button if there are any other groups in the **In groups** listbox, remove them

Click on the **Add** button at the bottom. Notice that the hostname used here, `snmptraps`, must be the same as the one we configured in the `snmptrap.sh` script; otherwise, the traps won't be received in Zabbix.

Now, click on **Items** next to the **snmptraps** host, and then click on **Create item**. Enter these values:

+   - **Name:** `Received SNMP traps`
+   - **Type:** **Zabbix trapper**
+   - **Key:** `snmptraps`
+   - **Type of information:** **Character**

> We used the **Character** type of information here as our script is expected to pass less information to the item. If large amounts of information would have had to be passed, we would have set this parameter to **Text** again.

![](bbox=[0.174, 0.78, 0.221, 0.826])

When you are done, click on the **Add** button at the bottom. Again, notice how we used the exact same key spelling as in the `snmptrap.sh` script.