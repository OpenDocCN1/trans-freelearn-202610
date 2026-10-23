In the preceding figure, we have a breakdown as shown here:

| Flavor size | Instances | vCPUs | Memory (GB) |
|---|---|---|---|
| Small | 13 | 13 | 26 |
| Medium | 24 | 48 | 96 |
| Large | 4 | 16 | 32 |
| Total | 41 | 78 | 154 |
| Reserved for OS |  | 18(~20%) | 38 GB(~20%) |

This shows an almost ideal configuration for our earlier flavor requirements, even though we didn't fill it with *all* medium-sized images. However, if more flavors had been added without adhering to the recommended preceding model, we would end up with gaps and wasted CPU and memory depending on the flavor diversity and number of instances of those flavors.

Using this flavor determinism as a best practice will allow operators to more accurately measure/monitor the following:

+   - Overall capacity
+   - Growth over time
+   - Utilization and overutilization
+   - Correct VM placement by Nova

Many companies have spent considerable time developing in-house strategies for collecting, managing, and analyzing data from their clouds to answer the topics discussed. However, in recent years, many different Cloud Management Platforms(CMPs) and purpose-built monetization and management software have emerged to simplify chargeback and showback. However, there have been scale challenges around OpenStack's telemetry project Ceilometer. Advances in technology have been made; however, some capacity planning software is now configured to use the OpenStack APIs directly and does not even need the OpenStack telemetry service (Ceilometer).