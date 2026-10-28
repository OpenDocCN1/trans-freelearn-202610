In this case, the hostname would be taken from the configuration file for the first and the third entry, while still overriding that for the second entry.

> If the input file contains many entries, `zabbix_sender` sends them in batches of 250 values per connection.

When there's a need to send lots of values constantly, one might wish to avoid repeatedly running the `zabbix_sender` binary. Instead, we could have a process write to a file new entries without closing the file, and then have `zabbix_sender` read from that file. Unfortunately, by default, values would be sent to the server only when the file is closed—or every 250 values received. Fortunately, there's also a command line flag to affect this behavior. Flag `-r` enables a so-called **real-time mode**. In this mode, `zabbix_sender` reads new values from the file and waits for 0.2 seconds. If no new values come in, the obtained values are sent. If more values come in, it waits for 0.2 seconds more, and so on up to 1 second. If there's a host that's constantly streaming values to the Zabbix server, `zabbix_sender` would connect to the server once per second at most and send all the values received in that second in one connection. Yes, in some weird cases, there could be more connections—for example, if we supplied one value every 0.3 seconds exactly.

If sending a huge number of values and using a file could become a performance issue, we could even consider a named pipe in place of the file—although that would be a quite rare occurrence.

# Sending timestamped values

The data that we sent so far was considered to be received at that exact moment—the values had the timestamp assigned by the server when it got them. Every now and then, there's a need to send values in batches for a longer period of time, or import a backlog of older values. This can be easily achieved with `zabbix_sender`—when sending values from a file, it supports supplying a timestamp. When doing so, the value field in the input file is shifted to the right and the timestamp is inserted as the third field. For a quick test, we could generate timestamps 1, 2, and 3 days ago:

```
for i in 1 2 3; do date -d "-$i day" "+%s"; done
```

Take the resulting timestamps and use them in a new input file:

```
- room.persons 1462745422 11
"A test host" room.persons 1462659022 12
- room.persons 1462572622 13
```