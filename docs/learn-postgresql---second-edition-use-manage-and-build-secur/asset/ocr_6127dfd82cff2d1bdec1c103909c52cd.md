```
pid | 108
usesysid | 16384
username | replicarole
application_name | walreceiver
client_addr | 172.29.0.4
client_hostname | 
client_port | 43400
backend_start | 2023-06-20 16:10:30.779359+00
backend_xmin | 
state | streaming
sent_lsn | 0/3005948
write_lsn | 0/3005948
flush_lsn | 0/3005948
replay_lsn | 0/3005948
write_lag | 
flush_lag | 
replay_lag | 
sync_priority | 0
sync_state | async
reply_time | 2023-06-20 16:11:49.332655+00
```

On the physical replication server, we have:

```
postgres@pg_replica:~$ psql forumdb

forumdb=# select * from pg_stat_replication;
-[ RECORD 1 ]---------------------------------------------
pid | 101
usesysid | 16468
username | logicalreplicarole
application_name | users_sub
client_addr | 172.29.0.2
client_hostname | 
client_port | 43950
backend_start | 2023-06-20 16:10:39.685477+00
backend_xmin | 
state | streaming
sent_lsn | 0/3005948
```