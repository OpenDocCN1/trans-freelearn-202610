

The idea is to track a group of queries that have the same normalized text so that you can get an idea of how many times such a group has been executed, even if with different arguments.

The `pg_stat_statements` special view keeps track of the most frequently executed queries up to the value of the configuration parameter `pg_stat_statements.max`, which defaults to 5000. Once the limit is reached, the least executed queries will be discarded in favor of fresh new ones. This ensures that the space occupied by the `pg_stat_statements` table will remain pretty much constant without any regard to the number of executed statements.

The `pg_stat_statements` view provides many fields that cannot be discussed in detail here, ranging from planning time to buffers and I/O activity. This extension is very useful when you want to deal with the workload of your cluster.

## Resetting data collected from pg_stat_statements

It is possible, at any given time, to reset all the data that’s been collected by the extension that’s invoking the `pg_stat_statements_reset()` function as a database administrator. The function will erase all the data that’s been collected and will allow the extension to collect new data from scratch. This can be useful when you want to test new configurations or hardware without having the collected data be biased due to old statistics:

```
```sql
formundb=# SELECT pg_stat_statements_reset();
```
```

By default, pg_stat_statements data is kept across clean database shutdowns and restarts.

## Tuning pg_stat_statements

The extension allows database administrators to limit the amount of data that’s collected. In particular, you can tune the following parameters in your postgresql.conf configuration file:

- `pg_stat_statements.max` indicates the maximum number of individual queries to collect.
- `pg_stat_statements.save` is a Boolean that indicates whether the content of the collected data must survive a clean system reboot. By default, this setting is true.
- `pg_stat_statements.track` allows you to specify the nesting level to track. With the top value, the extension will collect data about the query that was issued directly within clients and within tracking nested statements. This is triggered by the execution of other statements (for example, in function statements). With the value of all, the extension will trigger every statement and its descendants, while with none, no data will be collected about user statements.
- `pg_stat_statements.track_utility` tracks all statements that are not in SELECT, INSERT, UPDATE, or DELETE – in other words, “non-ordinary” statements. By default, this setting is on.

Usually, you don’t have to tune these settings since `pg_stat_statements` comes already configured to track what most use cases need.

## Summary

In this chapter, you learned how PostgreSQL manages configuration through a main text file, `postgresql.conf`, that can be split into smaller files, including the automatically loaded `postgresql.auto.conf`, which is always loaded at the end of the configuration process. Every configuration option can be edited in the configuration file and can be inspected within the database thanks to dedicated system catalogs. This allows the database administrator to not only have a clear understanding of the currently running configuration but to also search for configuration errors and incorrectly loaded settings.

PostgreSQL also collects *statistics*; that is, runtime data that was gathered during the cluster’s operational time. Those statistics can help an administrator understand what is going on, or what happened in the near past, in the cluster. Thanks to a different set of catalogs, which was exposed in this chapter, you learned how to dig into the details of all the information that PostgreSQL has collected for you. Being able to track and analyze what single applications, users, and connections are doing in a specific moment against the cluster provides database administrators with a great way to fix bottlenecks and other problems, thus helping to improve the cluster experience.

Finally, you learned about the `pg_stat_statements` extension, thanks to which it is possible to collect historical data about query execution and timing so that it is possible to apply optimization and deep analysis of the cluster activity.

Now that you’ve understood how to configure and monitor your cluster, it is time to learn how to replicate this. The next chapter will show you how to perform physical replication by configuring the cluster appropriately.

## Verify your knowledge

- What is a configuration context?

A configuration context defines how the cluster will perceive changes to a configuration parameter – for example, only at boot time or at the next incoming connection. See the *Configuration contexts* section for more details.

- What is the difference between the catalogs pg_settings and pg_file_settings?

The pg_settings catalog shows the values of every configuration parameter, as well as its admitted and valid values; the pg_file_settings catalog shows where (i.e., in which file and at which line) a configuration parameter has been found and loaded. See the *Inspecting all configuration parameters* section for more details.

- Besides editing configuration files, how can you modify the cluster configuration via SQL statements?

You can issue an ALTER SYSTEM command to change the values of a configuration setting. Changes will be written into the postgresql.auto.conf file. See the *Modifying the configuration from a live system* section for more details.

- How can you get information about running connections, transactions, and queries?

The special catalog pg_stat_activity provides information about every single backend process, its running (or last run) query, and its transaction state. See the *Information about running queries and connections* section for more details.

- What does the pg_stat_statements extension do?

The pg_stat_statements extension provides a historical view of the most commonly repeated queries, with information about the running times, the number of executions, and other details. See the *Advanced statistics with pg_stat_statements* section for more details.

## References

- PostgreSQL cluster configuration, official documentation: https://www.postgresql.org/docs/current/runtime-config.html
- PGConfig online configurator: https://www.pgconfig.org/
- PostgreSQL statistics collector official documentation: https://www.postgresql.org/docs/current/monitoring-stats.html
- PostgreSQL pg_stat_statements official documentation: https://www.postgresql.org/docs/current/pgstatstatements.html

## Learn more on Discord

To join the Discord community for this book – where you can share feedback, ask questions to the author, and learn about new releases – follow the QR code below:

https://discord.gg/jYWCjF6Tku

![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_637_0.png)

## 17

## Physical Replication

When a database, after passing the development and testing phases, arrives in production, the first problem that the DBA must address is managing replicas. Replicas must be managed in real time and automatically updated. Replicas allow us to always have a copy of our data updated in real time on another machine. This machine can be placed in the same data center as our data or in a different one. This chapter differs from all that we have seen previously in that we will talk about physical replication. In PostgreSQL, starting from version 9.x, it is possible to have physical replication natively. We will talk about what physical replication means, and we will see how to create a replica server and how to manage it. We will also see that it is possible to have synchronous or asynchronous replicas and that there can be multiple replicas of the same database, as well as the possibility of having replicas in a cascade.

In this chapter, we will return to the topic of WAL, something we have already discussed in Chapter 11, Transactions, MVCC, WAL, and Checkpoints. In order to execute the commands that will be shown in this chapter, we need to install a PostgreSQL server on two machines, or install two instances of PostgreSQL on the same machine but running on different ports. In the rest of the chapter, it will be presumed that you have two PostgreSQL installations available on different machines, to better simulate the situation of a real production environment, and starting from this chapter, we will learn how to install and configure a physical replication.

