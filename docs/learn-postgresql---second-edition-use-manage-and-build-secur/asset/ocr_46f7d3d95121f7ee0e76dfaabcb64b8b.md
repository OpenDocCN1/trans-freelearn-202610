Thus, we have successfully prepared our logical replica. We will now learn how to monitor it in the next section.

# Monitoring logical replication

Just as it does for physical replication, PostgreSQL provides the necessary tools to monitor logical replication.

For logical replication, we must query the pg_stat_replication table, which is the same table used to monitor physical replication, as we can see here:

```
db_source=# \x
Expanded display is on.
db_source=# select * from pg_stat_replication ;
-[ RECORD 1 ]---------------------------------------------
pid              | 144
usesysid         | 16477
username         | replicarole
application_name | sub_all_tables
client_addr      | 192.168.144.2
client_hostname  | 
client_port      | 43162
backend_start    | 2023-06-16 15:04:09.074749+00
backend_xmin     | 
state            | streaming
sent_lsn         | 0/1DD0398
write_lsn        | 0/1DD0398
flush_lsn        | 0/1DD0398
replay_lsn       | 0/1DD0398
write_lag        | 
flush_lag        | 
replay_lag       | 
sync_priority    | 0
sync_state       | async
reply_time       | 2023-06-16 15:05:23.524003+00
```

The information shown by this query is the same as what we saw in the case of physical replication, but we know this information refers to a logical replica because we have the slot name sub_all_tables, which we created before, on the application_name.