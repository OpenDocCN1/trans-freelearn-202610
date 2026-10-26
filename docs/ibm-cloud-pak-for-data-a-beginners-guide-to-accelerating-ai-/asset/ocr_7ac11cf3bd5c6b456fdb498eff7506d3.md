The following diagram illustrates distributed processing with Cloud Pak for Data using the Hadoop Integration service to push the processing closer to where the data resides:

![](bbox=[0.13, 0.2, 0.88, 0.48])

Figure 5.22 – Distributed processing with Hadoop Integration

The Edge Analytics service in Cloud Pak for Data enables the following use cases:

+   - **Faster insights and actions**: When there is a need for detecting situations and taking action in real time, a centralized analytics approach might not be the most effective as it adds too much network latency for immediate time-critical action to be taken.

+   - **Continuous operations**: Moving analytics to the edge, close to the device, also enables support for devices that must continue to provide critical functions even if disconnected from the **wide-area network (WAN)**/data center/cloud.

+   - **Better data and cost control**: With a massive growth in data volumes generated at the edge, outside the traditional data center there is a need to cost-effectively collect and analyze that data. Edge Analytics allows for filtering data at the source, aggregating data at the source where a finer level of detail is not required, and processing sensitive data at the edge.