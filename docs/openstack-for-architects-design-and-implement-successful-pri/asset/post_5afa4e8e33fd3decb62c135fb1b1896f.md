
This variability of demand is typically linear and possibly exponential based on cloud adoption. However, proper methodologies are required when constructing an OpenStack environment to help develop the most accurate capacity planning and provide a foundation to create strategic and tactical decisions later. This differs quite a bit from virtualization capacity planning, which tends to be more linear since the workloads are more persistent and are not recycled very frequently.

## Planning your city

Very similar to civil planning and architecture, the OpenStack cloud is a city of tenants that require resources to exist. However, with limited resources, tenants cannot be left to self-manage; the risk of resource exhaustion is too great. This would be analogous to the residents of an apartment building having unlimited power, water, gas, cable, and space without metering or charging for anything. Many residents would probably operate under the assumption that the resources were unlimited and they could oversize and oversubscribe to everything without consequence. Therefore, it is up to operations, both in civil engineering and OpenStack clouds, to do capacity management and risk mitigation for the end user. This means finding a way to track, analyze, and gather payment (or at least show costs) from end users or their tenant cost center. This includes creating packages of structured resources that are predetermined, which users can select from as well as creating and maintaining a quota system.

Capacity risk mitigation and planning are done by performing three distinct actions:

- Track usage
- Analyze growth
- Chargeback/showback

## Tracking usage and analyzing growth

In order to track usage of any resource, we first need to standardize the units we plan to count. In cloud computing, extreme granularity is possible using the data that exists in OpenStack databases, as well as using the metering project Ceilometer. While there are many different meters that can be tracked, let's start by tracking the number of individual vCPUs used over time, amounts of memory over time, and disk usage down to the minute. However, letting users put together their own combinations of these three data points can result in tens of thousands of combinations. Therefore, in the OpenStack cloud, we use flavors to limit user choices. Flavors are virtual hardware templates that are logical groupings of compute, storage, and even network elements.

This simplifies the chargeback/showback equation by logically grouping resources into manageable offerings. In our examples, we will only be considering vCPU and memory; however, the same principles can be applied to persistent and ephemeral storage as well as network resource allocation.

Continuing with our analogy of civil engineering, we can think of a flavor as an extended family looking to occupy an apartment building with a set of space requirements. In this example, one constraint we will have is that the 65,536 sq/ft building can only have a maximum of 16 rooms and by code, can only hold 64 people. However, since this is a new apartment building, the room sizes have not been determined and the property manager will build the rooms to match the family's requirements. The property manager decides to let the first three applicants pick sizes for their rooms and he will bill them for whatever specifications the families need.

Let's start with each family's requests, each with exact and unique requirements:

- 3-person family requiring 1,024 square feet
- 7-person family requiring 8,096 square feet
- 29-person family requiring 30,720 square feet

Below we can see what a sample 65,536 sq/ft house would look like using these unique and arbitrary room sizes:

![](img/2f752555f69992669defa8b25e6dcee9_126_0.png)

We now have a total of 49 people and their belongings occupying the building with a capacity of 64. That leaves room for 15 more people spread across a lot of wasted space (resources) of all types and configurations since no more of the existing family groupings listed above fit nicely into equal rooms in the existing space; the property manager only has two choices. He can create additional odd-sized apartments to fit 15 more people of different families and hope that families of that size apply, or buy a new house and continue to use the existing odd sizes while abandoning the wasted space in the property. This is what happens in an OpenStack cloud when flavor standardization does not take place.

Consider a compute node as a cargo van and flavors as boxes. With standard size boxes come ease of packing and high efficiency. Consider the boxes as vCPUs; for now, we will consider the boxes all square with the following measurements:

- 1-ft wide box
- 2-ft wide box
- 4-ft wide box

Most of the boxes people ship on this route are of the 2-ft flavor, but two other types of boxes are also available (1 foot and 4 foot). However, the Department of Transportation only allows 64 feet of boxes per truck. However, this time we created spaces that were stackable. We can now see what happens when we have structured groupings of boxes that are exponents of one another:

![](img/2f752555f69992669defa8b25e6dcee9_127_0.png)

In order to determine compute capacity, we use the desired VM density that is desired; for this discussion we will set the estimate at 40 VMs per compute server and multiply the density by the organization's most common flavor:

40 x 2 vCPU = 80 vCPU (assuming non-hyperthreaded CPU, multiply this number by 1.3 for hyperthreading)

40 x 4096 = 160 GB RAM (assuming a 1:1 ratio of RAM subscription)

As a rule of thumb, compute nodes require about 20% of the total vCPU and RAM for overhead and OS processes when fully loaded. These resources need to be included in the calculations for total resources needed:

Required number of vCPUs with OS overhead: 80 * 1.20 (100% + 20%) = 96 vCPU

Required memory with OS overhead: 160 * 1.20 = 192 GB RAM

Therefore, in order to have an accurately scaled starting point, we would need a compute server with the hardware to support 96 vCPUs. We will set the overcommit ratio at 8:1 for vCPUs, therefore we would need to divide the number of required vCPUs by 8 to get the number of the CPU cores we need. Overcommit ratios may vary, so adjust your calculation to match, as shown here:

Total number of vCPUs needed / Overcommit = Cores

96 / 8 = 12 CPU cores

Therefore, based on this calculation, we need 12 CPU cores for about 40 instances of the average flavor of this cloud, named medium. Since most servers come as dual-core or quad-core models now, we would simply need a single dual core CPU with a 6-core processor. For this example, something like the Intel E5-2620v3 or E5-2643v3 would be sufficient for generalized workloads.

For memory, we simply need to ensure that each compute server has 192 GB of RAM, and since this is a 6-dual-core single CPU server with 12 DIMM banks, each bank would require 32 GB per processor bank. Therefore, as a visual representation, the following figure is roughly what the logical memory allocation could look like.

## Building to Operate

There are some spare resources remaining after the server has reached its 40-instance limit plus one. This memory could be used for an additional instance or left as a reserve for compute server OS spikes at a load.

| Compute server - 192GB RAM - 2 x 2643 Intel CPU (96 vCPUs @ 8:1 OverSub) |
|---|
| S | S | L | M | M | L |
| S | S | L | M | M | L |
| M | S | S | M | S | S | S | S |
| M | S | S | M | S | M |
| L | M | M | L | M | M |
| M | M | M | M |
| M | M | M | M |
| OS Reserved CPU |

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

- Overall capacity
- Growth over time
- Utilization and overutilization
- Correct VM placement by Nova

Many companies have spent considerable time developing in-house strategies for collecting, managing, and analyzing data from their clouds to answer the topics discussed. However, in recent years, many different Cloud Management Platforms(CMPs) and purpose-built monetization and management software have emerged to simplify chargeback and showback. However, there have been scale challenges around OpenStack's telemetry project Ceilometer. Advances in technology have been made; however, some capacity planning software is now configured to use the OpenStack APIs directly and does not even need the OpenStack telemetry service (Ceilometer).
