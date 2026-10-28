# Acting upon Monitored Conditions

Sometimes, the receiver of the message might benefit from additional information that's, not directly obtainable from event-related macros. Here, an additional class of macros helps — the ones used in trigger expressions also work for macro contents. Imagine a person managing two servers that both rely on an NFS server, which is known to have performance problems. If the system load on one of these two servers increases enough to fire a trigger, the alert receiver would want to know the load on the second server as well, and also whether the NFS service is running correctly. That would allow them to do a quick evaluation of where the problem most likely lies — if the NFS service is down or is having performance problems of its own, then the system load on these two servers most likely has risen because of that, and the NFS server admin will have to take care of that. For this person to receive such information, we can add lines these to the e-mail body:

```
CPU load on Another host: {Another host:system.cpu.load.last()}
NFS service is: {NFS Server:nfs.service.last()}
```

![](bbox=[0.147, 0.387, 0.236, 0.434])

> Make sure to adjust item intervals and trigger expressions to avoid race condition for these items.

Note, there is no built-in NFS service item—one has to create proper hosts and items to be able to reference them like this.

As can be seen in the example, the same syntax is used as in trigger expressions, including the functions supported. This also allows the receiver to be immediately informed about average load over a period of time by adding a macro such as this:

```
Average CPU load on Another host for last 10 minutes: {Another host:system.cpu.load.avg(600)}
```

You can find a full list of supported macros in the official Zabbix documentation at https://www.zabbix.com/documentation/3.0/manual/appendix/macros/supported_by_location.

# Sending recovery messages

The setup we used only sent out messages when the problem happened. That was ensured by the `Trigger value = PROBLEM` condition, which was added by default. One way to also enable the sending of messages when a trigger is resolved would be to remove that condition, but it will not be useful when escalation functionality is used. Thus it is suggested to leave that condition in place and enable recovery messages on the action level instead.