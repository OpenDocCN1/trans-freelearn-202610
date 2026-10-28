The first thing is the item to collect: the current time. Go to **Configuration | Hosts**, click on **Items** next to **Another host**, and then click on **Create item**. Fill in the following values:

+   - **Name:** `Local time`
+   - **Key:** `system.localtime`
+   - **Units:** `unixtime`

When you are done, click on the **Add** button at the bottom. This item returns the current time as a Unix timestamp. While a unit is not required for our trigger, we entered unixtime there. This will translate the timestamp to a human-readable value in the frontend. We discussed item units in more detail in *Chapter 3, Monitoring with Zabbix Agents and Basic Protocols.*

In the bar above the item list, click on **Triggers**, then click on the **Create trigger** button. Enter these values:

+   - **Name:** `Incorrect clock on {HOST.NAME}.`
+   - **Expression:** Click on **Add** and then on **Select** next to the **Item** field. In the item list for **Another host**, click on **Local time** in the **NAME** column and click on **Insert**. The **Expression** field is filled with this expression: `{Another host:system.localtime.last()}=0`. This isn't actually what we need, but we tried to avoid the function dropdown here, so we will edit the expression manually. Change it to read this: `{Another host:system.localtime.fuzzytime(30)}=0`.
+   - **Severity:** Select Warning.

When you're done, click on the **Add** button at the bottom. The `fuzzytime()` function accepts a time period as a parameter. This makes it compare the timestamp of the item with the current time on the Zabbix server. If the difference is bigger than the time specified in the parameter, it returns 0, which is the problem condition we wanted to catch. Again, if you are not sure about the return value of some trigger function, better check the Zabbix manual.

> Don't forget that an incorrect time on the Zabbix server can result in a huge number of alerts about all other systems.