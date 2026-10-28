With a file named `sender_input_timestamps.txt`, we would additionally use the `-T` flag to tell zabbix_sender that it should expect the timestamps in there:

```
```bash
$ zabbix_sender -c /usr/local/etc/zabbix_agentd.conf -T -i /tmp/sender_input_timestamps.txt
```
```

All three values should be sent successfully.

> When sending in values for a longer period of time, make sure the history and trend retention periods for that item match your needs. Otherwise, the housekeeper process could delete the older values soon after they are sent in.

Looking at the graph or latest values for this item, it is probably slightly messed up. The timestamped values we just sent in are likely to be overlapping in time with the previous values. In most cases, sending in values normally and with timestamps for the same item is not suggested.

> If the Zabbix trapper items have triggers configured against them, timestamped values should only be sent with increasing timestamps. If values are sent in a reversed or chaotic older-newer-older order, the generated events will not make sense.
> 
> If data is sent in for a host which is in a no-data maintenance, the values are also discarded if the value timestamp is outside the current maintenance window. Maintenance was discussed in *Chapter 5, Managing Hosts, Users, and Permissions.*

# SSH and Telnet items

We have looked at quite a lot of fairly custom and customizable ways to get data into Zabbix. Although external checks should allow us to grab data by any means whatsoever, in some cases we might need to collect data from some system that is reachable over SSH or even Telnet, but there is no way to install an agent on it. In that case, a more efficient way to retrieve the values would be to use the built-in SSH or Telnet support.