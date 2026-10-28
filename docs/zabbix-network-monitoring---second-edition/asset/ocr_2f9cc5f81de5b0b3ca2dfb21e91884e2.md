With the problem—a missing host or host group—solved, let's click on **Add** again. The maintenance entry should appear in the list:

| NAME | TYPE | ACTIVE SINCE | ACTIVE TILL | STATE | DESCRIPTION |
|---|---|---|---|---|---|
| Normal maintenance | With data collection | 2016-03-23 00:00 | 2017-03-24 00:00 | Active | We keep data during this maintenance |

> The reminder to click on the small Add link was not repeated several times for no reason—it is too easy to forget to click on it and actually miss your changes in some cases. For example, if you were adding the second subperiod and forgot to click on the small link, it would be silently discarded. Watch out for similar traps in other forms.

With the maintenance entry added, let's try to see the effect this has on several sections in the frontend. In the console, run this:

```
$ cat /dev/urandom | md5sum
```

Navigate to **Monitoring | Triggers** and wait for the trigger to fire. When it shows up, look at the **HOST** column—this time, there's an orange wrench indicator. This shows us that maintenance is currently active for this host. Move the mouse cursor over this indicator:

![](bbox=[0.13, 0.521, 0.871, 0.676])

> You may click on the indicator to keep the message open, as with other popup messages in Zabbix.