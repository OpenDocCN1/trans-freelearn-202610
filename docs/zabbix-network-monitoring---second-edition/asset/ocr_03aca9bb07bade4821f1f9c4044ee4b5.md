You can use the item cloning feature to create the remaining two items more rapidly.
Use the Applications field instead of the New application field for the remaining items:

+   - **Name:** Warehouse analytics service
+   - **Type:** Zabbix trapper
+   - **Key:** warehouse_analytics
+   - **Application:** IT services

And for the last item:

+   - **Name:** Ticketing service
+   - **Type:** Zabbix trapper
+   - **Key:** ticketing
+   - **Application:** IT services

The final list of items should look like this:

| NAME ▲ | TRIGGERS | KEY | INTERVAL | HISTORY | TRENDS | TYPE | APPLICATIONS |
|---|---|---|---|---|---|---|---|
| Code repository service |  | code_repo | 90d | 365d | Zabbix trapper | IT services |
| Ticketing service |  | ticketing | 90d | 365d | Zabbix trapper | IT services |
| Warehouse analytics service |  | warehouse_analytics | 90d | 365d | Zabbix trapper | IT services |

Now click on Triggers in the navigation bar above the item list, then click on Create trigger. Create three triggers with settings as follows. For the first trigger:

+   - **Name:** Code repository down
+   - **Expression:** {IT services:code_repo.last()}=0
+   - **Severity:** High

For the second trigger:

+   - **Name:** Warehouse analytics down
+   - **Expression:** {IT services:warehouse_analytics.last()}=0
+   - **Severity:** High

And for the third trigger:

+   - **Name:** Ticketing down
+   - **Expression:** {IT services:ticketing.last()}=0
+   - **Severity:** High