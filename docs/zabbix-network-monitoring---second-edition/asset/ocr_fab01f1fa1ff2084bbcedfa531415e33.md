So this item will be recording the time when the proxy last contacted the Zabbix server. That's great, but hardly enough to notice problems in an everyday routine—we already know quite well that a trigger is absolutely needed. Here, the already familiar fuzzytime() function comes to the rescue. Navigate to Configuration | Hosts, click on Triggers next to the host you created the proxy last access item on, then click on the Create trigger button.

Let's say we have a fairly loaded and critical proxy—we would like to know when three minutes have passed without the proxy reporting back. In such a case, a trigger expression like this could be used:

```
{host:zabbix[proxy,proxy name,lastaccess].fuzzytime(180)}=0
```

> One could consider using the delta Store value for the last access item, which would return 0 when the proxy was not communicating. The trigger for such an item is more obscure, thus fuzzytime() is the most common trigger function for this purpose.

![](bbox=[0.12, 0.38, 0.18, 0.45])

As we might recall, the proxy connected to the server in two cases—it either synchronized the configuration, or sent the collected data. What if, for some reason, all occurrences of both of these events are further apart than three minutes? Luckily, the Zabbix proxy has a heartbeat process, which reports back to the server at regular intervals. Even better, this timing is configurable. Again, take a look at zabbix_proxy.conf, this time looking for the HeartbeatFrequency variable, which by default looks like this:

```
# HeartbeatFrequency=60
```

Specified in seconds, this value means that every minute the proxy will report back to the server, even if there are no new values to send. The lastaccess item is quite a reliable way to figure out when a proxy is most likely down or at least inaccessible, even if it would not be sending data for a longer period of time.

For our trigger, fill in the following values:

+   - Name: Proxy "First proxy" not connected for 3 minutes
+   - Expression: {Another host:zabbix[proxy,First proxy,lastaccess].fuzzytime(3m)}=0
+   - Severity: High

> Replace the proxy name with the host name on which the proxy last access item was created. If the last access item used the {HOST.HOST} macro, use the same macro in the trigger name and expression, too.

![](bbox=[0.12, 0.87, 0.18, 0.92])