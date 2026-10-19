# Monitoring

After a workflow has been triggered, each action in the workflow needs to be closely monitored. Monitoring should be set up and configured in such a way that it stores the state of each action for every pipeline run in the form of logs. The operational team should be alerted if any action failed so that corrective actions can be performed.

Monitoring data pipelines has several benefits, including the following:

+   - It helps to establish the overall health of the data pipeline running, and its success or failure.
+   - It helps establish the total runtime for the data pipeline. This information is critical for calculating the miss rate for **service-level agreements (SLAs)**.
+   - It helps identify performance bottlenecks. Having this information helps data engineers fine-tune code and manage resources.

> **Important note**
> In a data pipeline monitoring system, some key metrics to look out for are latency, throughput, and errors.

The understanding of what each component does in isolation, and how it contributes to the overall success of the data pipeline helps data engineers conceptualize, design, and develop components with a great degree of efficiency.

In the following sections, we will describe the various phases of the data pipeline creation process. In addition to the overall understanding of each phase, we will additionally learn about recommended tasks and actions that need to be accounted for. This list will hopefully give readers a solid baseline that can be further enhanced and built upon during real projects.

# Process of creating a data pipeline

In analytics-centric organizations, it is very common to have multiple data pipelines, each one addressing a different use case. To make matters worse, each use case may be owned by a different sub-group within the organization and require a different dataset. In such cases, it becomes extremely important to carefully plan and design the data pipeline operation so that efficiencies can be discovered and repetitive work can be avoided. The creation of data pipelines is done in phases. In the subsequent sections, we will learn about each phase separately.