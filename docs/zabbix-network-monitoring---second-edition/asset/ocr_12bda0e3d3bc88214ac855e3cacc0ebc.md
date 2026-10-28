Note that there is no way to query the current log level or set a specific level. If you are not sure about the current log level of all the processes, there are two ways to sort it out:

+   - Restart the daemon
+   - Decrease or increase the log level five times so that it's guaranteed to be at 0 or 5, then set the desired level

As a simple test of the options we just explored, increase the log level for all pollers:

```
# zabbix_server --runtime-control log_level_increase=poller
```

Follow the Zabbix server logfile:

```
# tail -f /tmp/zabbix_server.log
```

Notice the amount of data just 5 poller processes on a tiny Zabbix server can generate. Then decrease the log level:

```
# zabbix_server --runtime-control log_level_decrease=poller
```

# Runtime process status

Zabbix has another small trick to help with debugging. Run top and see which mode gives you a more stable and longer list of Zabbix processes—one of sorting by processor usage (hitting Shift + P) or memory usage (hitting Shift + M) might.

> Alternatively, hit o and type COMMAND=zabbix_server.

Press C and notice how Zabbix processes have updated their command line to show which exact internal process it is and what is it doing:

```
zabbix_server: poller #1 [got 0 values in 0.000005 sec, idle 1 sec]
zabbix_server: poller #4 [got 1 values in 0.000089 sec, idle 1 sec]
zabbix_server: poller #5 [got 0 values in 0.000004 sec, idle 1 sec]
```

Follow their status and see how the task and the time it takes changes for some of the processes. We could also have output that could be redirected or filtered through other commands:

```
# top -c -b | grep zabbix_server
```