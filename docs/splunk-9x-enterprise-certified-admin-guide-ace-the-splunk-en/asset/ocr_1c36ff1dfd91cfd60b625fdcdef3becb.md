+   - `maxTotalDataSizeMB` is the total index size limit. If you look at the example index, `windows-event-logs`, we have set this value to 500 MB.
+   - Global settings are applied to every index specified in `indexes.conf`. For a complete list, refer to the `indexes.conf.spec` file in the Splunk docs. At the global level, the `lastChanceIndex` option can be configured. This acts as a final option for any external inputs that don't indicate a valid index. If this value is left empty, Splunk will simply discard these events.

What if the data moved to cold buckets is not being searched very frequently, so the allocation of expensive hardware storage similar to hot/warm buckets isn't cost-effective? Splunk suggests using low-latency high-throughput storage for hot/warm buckets as they hold recent data that is searched quite often. Cold buckets can reside on standard latency storage, which is slow and cheap. To achieve this, an index can be configured using volumes. Let's look at volumes in the next section.

# Index volumes

Volumes are specific directories on a filesystem that are allocated for indexes in order to manage storage space effectively. Let's take a look at the volume configuration in a UNIX-based OS for the `windows-event-logs` index that we created on Splunk Web in the previous section:

```
[volume:hot_storage]
path = /mnt/fast_disk
# Optional limits the volume size to 60 GB
maxVolumeDataSizeMB = 61440

[volume:cold_storage]
path = /mnt/slow_disk
#Optional limits the volume size to 50 GB
maxVolumeDataSizeMB = 500

#index definition
[windows-event-logs]
homePath = volume:hot_storage/windows-event-logs
coldPath = volume:cold_storage/windows-event-logs
maxTotalDataSizeMB = 51200
thawedPath = $SPLUNK_DB\windows-event-logs\thawedd b
```

This configuration contains the `volume` keyword in `homePath` and `coldPath`. `thawedPath` is set to the standard location as it cannot be on a volume. If you would like to know some more advanced options, then look at the `indexes.conf.spec` file in the `$SPLUNK_HOME/etc/system/README` directory.

In the next section, we will go through backing up indexes.