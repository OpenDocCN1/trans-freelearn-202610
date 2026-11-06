+   - Your application’s average and peak write volume
+   - The expected maximum duration of potential outages
+   - Network reliability between data centers (for geographically distributed deployments)
+   - The number of secondary nodes (more secondaries may require a larger window)

MongoDB provides tools to monitor oplog usage and estimate your current window:

```
// Check oplog size and replication window
db.printReplicationInfo();

// Sample output:
// configured oplog size: 5120MB
// Log length start to end: 86400secs (24hrs)
// oplog first event time: Wed Apr 02 2025 10:03:25 GMT+0000
// oplog last event time: Thu Apr 03 2025 10:03:25 GMT+0000
// now: Thu Apr 03 2025 10:05:01 GMT+0000
```

The oplog boundaries can be set in one of two ways, size and time, as follows:

+   - size: The maximum size of the oplog in megabytes, which can be configured to any value between 990 MB and 1 petabyte
+   - minRetentionHours: The minimum number of hours to preserve an oplog entry

As the oplog is a capped collection, when these configured boundaries are met, the oldest entries in the oplog will be deleted.

Changing the configured size of the oplog from its current value to 16 GB could be done as follows:

```
db.adminCommand({ "replSetResizeOplog": 1, size: Double(16384)})
```

Similarly, if instead of a fixed size you wanted to ensure exactly 48 hours of oplog content was always available, the command would be the following:

```
db.adminCommand({ "replSetResizeOplog": 1, minRetentionHours: 48})
```

![](bbox=[0.14, 0.75, 0.22, 0.81])

# Risk of disk space exhaustion with minRetentionHours

When minRetentionHours is configured, the oplog can grow without constraint so as to retain oplog entries for the configured number of hours. This may result in a reduction or exhaustion of system disk space due to a combination of high write volume and large retention period.