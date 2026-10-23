+   - `primary_conninfo` and `primary_slot_name` are used to determine the connection from a standby node to a primary one.
+   - `hot_standby`, when used on a replicating system, allows for read-only queries.
+   - `max_standby_archive_delay` and `max_standby_streaming_delay` define the amount of time before a conflicting query on the standby has to be canceled due to some other action that happened on the primary.
+   - `recovery_min_apply_delay` introduces a delay on the standby node so that it can follow the primary with a timeshift.
+   - `max_replication_slots` and `max_wal_senders` are used to define how many replication slots will be accepted and in use and how many processes will manage replication.
+   - `synchronous_standby_names` defines which nodes are to be considered synchronous in replication, and therefore the primary has to receive constant feedback before applying changes.

These settings and other replication-related settings will be discussed in *Chapter 17, Configuration and Monitoring*, and *Chapter 18, Replication*. 

# Vacuum and autovacuum-related settings

There are different settings that can be used to define and tune the vacuum and autovacuum settings. These were discussed in *Chapter 11, Transactions, MVCC, WALs, and Checkpoints*. 

# Optimizer settings

The PostgreSQL optimizer is driven by a cost-based approach. It is possible to tune these costs, as discussed in *Chapter 13, Query Tuning, Indexes, and Performance Optimization*. 

# Statistics collector

PostgreSQL exploits the **statistics collector** to gather facts about what happened in the cluster, as you will learn later in this chapter in the *Monitoring the cluster* section.

Since collecting those numbers has little runtime impact, it is possible to exclude the collection entirely or filter the statistics collector to gather only the facts you are truly interested in. The main settings for this are as follows:

+   - `track_activities` enables other processes to monitor the current command or query currently being executed.
+   - `track_counts` gathers counting information about tables and index usage.