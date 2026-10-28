The macros for the hostname and IP address are useful when either could change and we would not want to check every map and then manually update those values. Additionally, when a larger amount of hosts is added to a map, we could do a mass update on them and enter "{HOST.NAME}" instead of setting the name individually for each of them.

> It's a good idea to save the map every now and then by clicking on the Update button — for example, now might be a good time to do so. Dismiss the strange popup by clicking on Cancel; the map was saved anyway.

Notice how we could also change the label position. By default, whatever is set in the map properties is used, but that can be overridden for individual elements.

There are more macros that work in element labels, and the Zabbix manual has a full list of those. Of special interest might be the ability to show the actual data from items; let's try that one out. In the label for A test host, add another line that says "{A test host:system.cpu.load.last()}" and observe the label:

![](bbox=[0.37, 0.62, 0.62, 0.68])

> There is no helper like in triggers — we always have to enter the macro data manually.
> If the label does not show the CPU load value but an *UNKNOWN* string, there might be a typo in the hostname or item key. It could also be displayed if there's no data to show for that item with the chosen function. If it shows the entered macro but not the value, there might be a typo in the syntax or trigger function name. Note that the hostname, item key, and function name all are case sensitive. Attempting to apply a numeric function such as avg() to a string or text item will also show the entered macro.

Real-time monitoring data is now shown for this host. The syntax is pretty much the same as in the triggers, except that map labels support only a subset of trigger functions, and even for the supported functions only a subset of parameters is supported. We may only use the trigger functions last(), min(), max(), and avg(). In the parameters, only a time period may be used, specified either in seconds, or in the user friendly format. For example, both avg(300) and avg(5m) would work in map labels.