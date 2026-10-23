```
client_addr | 192.168.122.11
client_hostname | 
client_port | 41690
backend_start | 2023-05-22 13:27:29.849+00
backend_xmin | 
state | streaming
sent_lsn | 0/3000148
write_lsn | 0/3000148
flush_lsn | 0/3000148
replay_lsn | 0/3000148
write_lag | 
flush_lag | 
replay_lag | 
sync_priority | 0
sync_state | async
reply_time | 2023-05-22 13:30:59.928374+00
```

Using this view, we have a lot of information that we need in order to know whether our stand_by server is in excellent health.

For example, we can see that the last reply message received from the replica server is 2023-05-22 13:30:59.928374+00, and we can see, thanks to the difference between the sent_lsn value and the replay_lsn value, that our replication server is perfectly aligned. For further information about pg_stat_replication, please refer to the official documentation (https://www.postgresql.org/docs/current/monitoring-stats.html#PG-STAT-REPLICATION-VIEW).

# Synchronous replication

So far, we have talked about asynchronous replication; this means that the primary server passes information to the replica standby without being sure that the standby server has replicated the data. In asynchronous replication, the primary server does not wait for the replica server to actually replicate the data. In synchronous replication, when the primary performs a commit, all the replicated servers synchronously commit. In synchronous replication, after the execution of the commit, we are sure that the data is replicated on the primary and all the replicas. When we want to achieve synchronous replication, it is good practice to have all identical machines and a good network connection between the machines; otherwise, performance can become slow.