There are many read preference modes available, which provide varying degrees of flexibility as it relates to maximizing availability and consistency, or minimizing latency.

| Read preference mode | Description |
|---|---|
| primary | The default read preference, where all operations read from the current replica set primary.

To maximize consistency and avoid stale reads, this read preference mode should be used. |
| primaryPreferred | In most situations, operations read from the primary, but if it is unavailable, operations read from secondary members.

This read preference mode allows for maximum availability, as consistent read operations will be delivered so long as a primary is available. If the primary is unavailable, however, you can still query secondaries. |
| secondary | All operations read from the secondary members of the replica set. |
| secondaryPreferred | Operations typically read data from secondary members of the replica set. If the replica set has only one primary member and no other members, operations read data from the primary member. |
| nearest | To minimize latency, operations read from a random eligible replica set member, irrespective of whether that member is a primary or secondary, based on a specified latency threshold. The operation considers the following when calculating latency:

- The localThresholdMS connection string option
- The maxStalenessSeconds read preference option
- Any specified tag set lists |

*Table 5.4: MongoDB read preference modes*

For applications that are geographically distributed, a read preference of nearest can further improve read performance as operations will be routed to any member with the smallest latency window, as defined by localThresholdMS (default 15 milliseconds).