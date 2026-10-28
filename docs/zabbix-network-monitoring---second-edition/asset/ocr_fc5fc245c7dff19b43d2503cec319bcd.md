Having hardcoded text can be useful, but showing monitoring data, like we did for a host, would be even better. Luckily, that is possible, and we could display network traffic data on this link. Change the link label to the following:

```
Incoming traffic: {A test host:net.if.in[eth0].last()}
Outgoing traffic: {A test host:net.if.out[eth0].last()}
```

> We cannot use automatic references such as {HOST.HOST} here. The link is not associated with any host and such a reference would fail.

We are mixing here both freeform text (you could label some link "Slow link", for example), and macros (in this case, referring to specific traffic items). Click on **Apply** for the link properties. This might also be a good moment to save the changes by clicking on **Update** in the upper right corner:

![](bbox=[0.314, 0.389, 0.687, 0.444])

Both macros we used and multiline layout work nicely.

> We can reference any item type—agent, SNMP, and others. As long as it's gathering data, values can be shown on a map.

For a full list of supported macros in map element labels, see the Zabbix manual.

# Reflecting problems on map elements

Having the problem count listed in the label is useful, but it's not that easy to see from a distance on a wall-mounted display. We also might want to have a slightly nicer looking map that would make problems stand out more for our users. Zabbix offers two methods to achieve this:

+   - Custom icons
+   - Icon highlighting

In the **First map** constructor, click on **A test host**. In the **Icons** section, choose a different icon in the **Problem** dropdown—for the testing purposes, we'll go with the **Crypto-router_(24)** icon, but any could be used. Click on **Apply**, then **Update** for the map. Additionally, run on "A test host":

```
$ rm /tmp/severity1
```