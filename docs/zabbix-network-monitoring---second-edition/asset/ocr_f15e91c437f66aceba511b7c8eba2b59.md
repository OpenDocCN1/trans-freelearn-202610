# Advanced Item Monitoring

When done, click on the **Add** button at the bottom. Some 3 minutes later, we could check the history for this item in the latest data page:

| TIMESTAMP | LOCAL TIME | VALUE |
|---|---|---|
| 2016-05-09 14:25:01 |  | 15445 |
| 2016-05-09 14:25:01 |  | 5445 |
| 2016-05-09 14:25:01 |  | 445 |

Hooray, extracting the values is working as expected. Writing triggers against them should be much, much easier as well. But one thing to note—for this item we were unable to see the graphs. The reason is the **Type of information** property in our log item—we had it set to **Log**, but that type is not considered suitable for graphing. Let's change it now. Go to **Configuration | Hosts**, click on **Items** next to **A test host**, and click on **Extracting log contents** in the **NAME** column. Change **Type of information** to **Numeric (unsigned)**, then click on the **Update** button at the bottom.

> 💡 If the extracted numbers have the decimal part, use **Numeric (float)** for such items.

Check this item in the latest data section—it should have a **Graph** link now. But checking that reveals that it has no data. How so? Internally, Zabbix stores values for each type of information separately. Changing that does not remove the values, but Zabbix only checks the currently configured type. Make sure to set the correct type of information from the start. To verify that this works as expected, run the following on "A test host":

```
```sh
$ echo "2016-12-21 18:16:13 Number of messages in the queue: 113" >> /tmp/zabbix_logmon/queue_log

$ echo "2016-12-21 18:17:14 Number of messages in the queue: 213" >> /tmp/zabbix_logmon/queue_log

$ echo "2016-12-21 18:18:15 Number of messages in the queue: 150" >> /tmp/zabbix_logmon/queue_log
```
```

Checking out this item in the **Latest data** section, the values should be there and the graph should be available, too. Note that the date and time in our log file entries still doesn't matter—the values will get the current timestamp assigned.

> 💡 Value extracting works the same with the **logrt** item key.