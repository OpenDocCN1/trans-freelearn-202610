# Available metrics

With some groups and hosts automatically created, let's see what data they are collecting. Navigate to Monitoring | Latest data and select Hypervisors in the Host groups field. Then, click on Filter:

| NAME ▲ | LAST CHECK | LAST VALUE |
|---|---|---|
| CPU (5 Items) |  |  |
| CPU cores | 2015-11-02 10:49:58 | 4 |
| CPU frequency | 2015-11-02 10:49:56 | 2.53 GHz |
| CPU model | 2015-11-02 10:49:57 | Intel(R) Xeon(R) CPU |
| CPU threads | 2015-11-02 10:49:59 | 8 |
| CPU usage | 2015-11-02 10:59:51 | 61 MHz |
| Datastore (2 Items) |  |  |
| Average read latency of the datastore datastore1 | 2015-11-02 10:59:44 | 0 |
| Average write latency of the datastore datastore1 | 2015-11-02 10:59:45 | 0 |

There will be more items for each hypervisor. Some of them might not have data yet, but a bit of patience should reveal all the details.

> Datastore items might appear later — they are discovered by the datastore discovery LLD rule in Template Virt VMware Hypervisor with a default interval of 1 hour.

Now, filter for a hypervisor virtual machine group in the Host groups field, or by a single discovered virtual machine in the Hosts field: