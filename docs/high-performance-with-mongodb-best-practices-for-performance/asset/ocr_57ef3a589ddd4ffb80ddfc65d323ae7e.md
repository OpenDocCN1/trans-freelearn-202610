# Configuring systems for MongoDB performance

In this section, we will look at actions you can take to diagnose and fix example issues with CPU cycles, memory, storage, and network.

# Understand the ideal ratio of simultaneous operations per CPU core

The MongoDB server runs multiple operations concurrently and distributes them across threads running on multiple CPU cores. As we saw in Chapter 1, Systems and MongoDB Architecture, the application server can become a bottleneck if it doesn’t send enough operations in parallel to fully utilize the MongoDB server.

For example, if you were to run a series of experiments using your application’s critical performance workload and chart the results in a spreadsheet, you might see a graph similar to Figure 13.2, which shows CPU utilization and performance trends as the number of threads or parallel operations increases.

# DB ops/s and CPU Utilization

![](bbox=[0.22, 0.52, 0.78, 0.74])

*Figure 13.2: The effects on CPU utilization and performance trends as you increase parallelism*

In this example, the database server is running on a 16-core CPU. Performance peaks at 64 threads, and may even decline slightly beyond that point as more threads are added. This is a typical pattern for many applications. In this case, optimal performance is reached with 64 threads on 16 cores, suggesting a ratio of 4 active threads or connections per core. It’s important to test with a production workload and dataset to determine the ideal threads-to-core ratio for your critical performance workloads. For instance, you don’t want to find yourself running only 8 operations in parallel on a 16-core machine, leaving half the cores idle, as this can lead to unnecessary resource costs.