+   - Being a single-threaded operation means the execution time is directly proportional to the data. Therefore, the growth of data typically means the process will take longer to finish. This could end up significantly impacting and/or delaying the decision-making process, therefore rendering the data analytics useless at times.
+   - Something as minor as a network glitch or machine failure requires the entire program cycle to be restarted, as illustrated in the following diagram:

![](bbox=[0.12, 0.25, 0.86, 0.48])

Figure 1.4 – Rise of distributed computing

The distributed processing approach, which I refer to as the paradigm shift, largely takes care of the previously stated problems. Instead of taking the traditional data-to-code route, the paradigm is reversed to code-to-data.

In a distributed processing approach, several resources collectively work as part of a cluster, all working toward a common goal. In simple terms, this approach can be compared to a team model where every team member takes on a portion of the load and executes it in parallel until completion.
If a team member falls sick and is unable to complete their share of the workload, some other member automatically gets assigned their portion of the load.

Distributed processing has several advantages over the traditional processing approach, outlined as follows:

+   - The code-to-data paradigm shift ensures the network does not get clogged. The entire idea of distributed processing heavily relies on the assumption that data is stored in a distributed fashion across several machines, also referred to as nodes. At the time of processing, only the code portion (usually a much smaller footprint as compared to actual data) is sent over to each node that stores the portion of the data being processed. This ensures that the processing happens locally on the node where the data is stored.