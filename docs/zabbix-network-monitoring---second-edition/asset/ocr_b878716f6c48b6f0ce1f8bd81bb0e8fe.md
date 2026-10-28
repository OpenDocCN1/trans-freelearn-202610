# Units

We previously configured units for some items, using values such as B or ms. While the effect was visible in the monitoring section quite easily, there are some subtle differences in the handling of different units.

Units is a freeform field. You can type anything in there, but some units will change their behavior when data is displayed:

+   - **B/Bps**: By default, when applying **K, M, G, T** and other unit prefixes, Zabbix will use a multiplier of 1,000. If the unit is set to **B** or **Bps**, the multiplier used will be changed to 1,024
+   - **s**: An incoming value in seconds will be translated to a human-readable format
+   - **uptime**: An incoming value in seconds will be translated to a human-readable format
+   - **unixtime**: An incoming Unix timestamp will be translated to a human-readable format

Interestingly, for our ICMP ping item, we did not use any of these; we used ms instead. The reason is that in certain cases of a very small roundtrip, a value in seconds might be too small to properly store in the Zabbix database schema. By applying the multiplier of 1,000 in the item configuration, we converted the incoming value in seconds to milliseconds, which should never exceed the limits of the database schema. One downside would be that if a ping takes a long time, the value will not be displayed in seconds—we will have to figure it out from the millisecond value.

> Units do not affect the stored values, only what gets displayed. We may safely change them back and forth until we get them right.

# Custom intervals

Another item property that we just briefly discussed was custom intervals. Most item types have their intervals configurable, which determines how often the item values should be collected. But what if we would like to change this interval based on the day of the week or the time of day? That is exactly what custom intervals enable us to do. There are two modes for custom intervals:

+   - Flexible intervals
+   - Custom scheduling