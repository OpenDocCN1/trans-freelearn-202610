The idea behind monitoring is two-fold.

First, it should supply you with information that your system is running as expected. Any errors, exceptions, network failures, database hiccups, and more should be identified, registered, and reported.

Secondly, monitoring should provide you with information about the usage of the system. How many new users do we have, how many orders are there in the system, how often does a certain REST endpoint access, and how often is a critical business method invoked?

The monitoring process consists of several steps:

+   1. Add annotations to your code that will actively monitor the metrics.
+   2. Expose the metrics data for harvesting.
+   3. Harvest the data.
+   4. Generate alerts and create visualized data.

In this section, we will discuss the first step and identify what we want to monitor and how to make the required code changes.

# Looking at the default metrics of a running system

Out of the box, MicroProfile Metrics already gathers a lot of information when it comes to the state of your application. It defines three categories of information:

+   1. System metrics.
+   2. Application metrics.
+   3. Vendor-specific metrics.

Let’s take a look at these metrics. We will use the Cargo Tracker application for this. Navigate to http://localhost:8080/metrics; you will see a list of all default metrics. Here is a part of the output:

```
# TYPE base_cpu_availableProcessors gauge
base_cpu_availableProcessors 10
# TYPE base_cpu_systemLoadAverage gauge
base_cpu_systemLoadAverage 5.9248046875
# TYPE base_gc_total_total counter
base_gc_total_total{name="G1 Young Generation"} 54
base_gc_total_total{name="G1 Old Generation"} 0
# TYPE base_jvm_uptime_seconds gauge
base_jvm_uptime_seconds 56.064
# TYPE base_memory_committedHeap_bytes gauge
base_memory_committedHeap_bytes 2.87309824E8
# TYPE base_memory_committedNonHeap_bytes gauge
```