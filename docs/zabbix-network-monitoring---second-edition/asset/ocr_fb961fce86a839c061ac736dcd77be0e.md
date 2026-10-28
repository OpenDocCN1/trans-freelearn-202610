For a passive agent, the server should query it directly. For an active agent, it should point at the Zabbix server. For the trapper item, zabbix_sender should be used to connect to the Zabbix server. In all three cases, the host should be assigned to be monitored by the Zabbix server. If we are using internal monitoring to collect proxy values in a dedicated host, a separate host will be needed to collect the buffer data. This way, we will avoid these values getting stuck in the proxy buffer.

For the agent items, we could use a UserParameter like this:

```
UserParameter=proxy.buffer,sqlite3 /tmp/zabbix_proxy.db "select (select max(proxy_history.id) from proxy_history)-nextid from ids where field_name='history_lastid';"
```

> You might have to use the full path to the sqlite3 binary.

As for the Zabbix trapper approach, it could be run from crontab or using any other method. The command would be similar to this:

```
zabbix_sender -z zabbix_server -s target_host -k item_key -o $(sqlite3 /tmp/zabbix_proxy.db "select (select max(proxy_history.id) from proxy_history)-nextid from ids where field_name='history_lastid';")
```

Here we use the basic zabbix_sender syntax, but the value is obtained from the SQLite query. See Chapter 11, Advanced Item Monitoring for more information on UserParameters and zabbix_sender. The Zabbix trapper item would receive the same data as the internal buffer monitoring – the buffer size. The trigger would check for this buffer exceeding some threshold.

Note that all three methods are likely to result in some missing values for the buffer item – the values would not be available while the connection between server and proxy is down. The active agent item approach would suffer less as it has in-memory buffer, but it there might still be missing values. If it would be valuable to know how the buffer changed during the communication breakdown; this item could be used for the trigger and an internal item, as discussed earlier, for more complete buffer statistics.

Regarding triggers and dependencies, it is suggested to make the buffer trigger depend on the last access trigger. This way, hosts behind the proxy will be silenced if the proxy disappears completely, and when the proxy comes back with a large buffer, the buffer trigger will keep those hosts silent.