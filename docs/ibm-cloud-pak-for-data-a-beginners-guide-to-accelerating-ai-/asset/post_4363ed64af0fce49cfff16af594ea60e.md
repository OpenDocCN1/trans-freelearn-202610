
As we have now explored what detecting AI bias brings to business operations, let’s next look into how data perturbation works and how the OpenScale service uses this technique as a differentiator to other options for bias detection.

## Data perturbation

The OpenScale service uses a data perturbation technique as a differentiator to other options for bias detection. Let’s use an example scenario of 100 insurance claims. In these claims, 60 claims were made by people in the 18-24 age group and 40 in the above 24 age group. All the people in the 18-24 age group had very high claim frequency and were known to have made fraudulent claims in the past, hence the model rejected all 60 claims in that age group. On the other hand, the 40 claims in the above 24 age group were all approved by the model. The disparate impact for this would be 0/1=0, leading to the model being flagged as biased. However, the model is not biased and is based on the data provided to it. To avoid this situation, OpenScale leverages data perturbation.

Data perturbation works by flipping the record in the monitored group with a random value from the reference group and then sends the perturbed value to the model. All the other features of the record are kept the same. Now, after perturbation, if the model predicts that the claim in the preceding example is approved, then it means the model is acting in a biased manner by making decisions just based on the age of the customer. Thus, while computing bias, we do this data perturbation and compute the disparate impact using the payload and the perturbed data. This will ensure that we report bias only if there are enough data points where the model has exhibited biased behavior across the original and the perturbed data. Thus, we are able to detect if there is genuine bias in the model and not get impacted by the kind of data being received by the model.

## Comparing model quality, fairness, and drift

The OpenScale service in Cloud Pak for Data enables you to monitor multiple models across different providers and algorithms, enabling customers to compare performance over a period of time and across different versions (production/pre-production).

LIME treats all models as black boxes and generates an explanation that is easy to understand. However, this also requires that LIME understands the model behavior in the vicinity of the data points. This can lead to increased costs because of more scoring requests. The OpenScale service has an innovative caching-based technique that leads to a significant drop in the number of scoring requests required for generating a local explanation. In addition to reducing cost, there is also a significant improvement in the speed of generating an explanation. The OpenScale service has also added a fault tolerance feature to cater to intermittent failure in scoring requests.

The second explainability algorithm provides contrastive explanations. This algorithm is tailored to handle models that accept structured data and is able to generate contrastive explanations with far fewer scoring requests compared to open source alternatives.

## Model drift

Model drift refers to a degradation in model performance due to changes in the underlying data and the relationship between I/O variables. Model drift can negatively impact over time, or sometimes there could be a sudden drift, causing undesirable model behavior. To effectively detect and mitigate drift, organizations need to monitor and manage model performance regularly as part of their data and AI platform. This integrated approach has the following benefits:

- Ability to track metrics on a continuous basis, with alerts on drifts in accuracy and data consistency.
- Ability to define targets and track them through development, validation, and deployment.
- Simplified steps to identify business metrics affected by model drift.
- Automating drift monitoring minimizes the impact of model degradation.

The OpenScale service detects drifts in accuracy and in the data. Estimating drifts in accuracy is done at model runtime. Model accuracy generally drops if there is an increase in transactions not covered by the model training dataset. This type of drift is calculated for structured binary and multi-class classification models only.

The following screenshot illustrates the ability to continually monitor models and evaluate model quality over time:

![](img/50e1dac0750b69f2b8e8efff000eb13b_134_0.png)

Figure 5.21 – Model evaluation over time

Now we have explored capabilities around predictive analytics, let's next look into how we can handle streaming data and perform analytics using the services in Cloud Pak for Data.

## Streaming data/analytics

Data is growing at a rapid pace, and the variety and complexity of data are continually increasing. Every day, we create 2.5 quintillion bytes of data, and traditional analytics solutions are not able to fully unlock the potential value of that data. Cloud Pak for Data has the IBM Streams service to address streaming data needs. This includes an API, an IDE for applications, and a runtime system that can run the applications on a single or distributed set of resources. The Streams service is designed to address the following data processing platform objectives:

- Parallel and high-performance streams processing
- Connectors to a variety of relational databases, non-relational data sources, and multiple streaming data sources including IBM Event Streams, HyperText Transfer Protocol (HTTP), and Internet of Things (IoT)
- Apply simple-to-advanced analytics methods, including ML, to real-time data streams
- Automated deployment of stream processing applications
- Incremental deployment without restarting to extend stream processing applications
- Secure and auditable runtime environment

There are several ways to develop applications that analyze streaming data, as outlined here:

- Streams flows from within a Cloud Pak for Data project
- Streams Python API from a Jupyter notebook in a Cloud Pak for Data project
- Streams Python API and any text editor or Python IDE

Now, let's move on to distributed processing.

## Distributed processing

The Cloud Pak for Data service leverages the Red Hat OpenShift platform to schedule workloads from different services across the compute nodes in a cluster. There are some situations where we might either not have capacity in a cluster or want to run a workload at a remote location, either due to data locality or regulatory requirements that dictate where the workload can be executed geographically. Cloud Pak for Data has the following two options for distributed processing:

- The Execution Engine for Apache Hadoop service
- The Edge Analytics service

The Execution Engine for Apache Hadoop service enables you to create Hadoop environment definitions and **Jupyter Enterprise Gateway (JEG)** sessions from within Watson Studio analytics projects to run jobs remotely on the Hadoop cluster. The following tasks can be accomplished using this service:

- Train a model on a Hadoop cluster
- Manage a model on a Hadoop cluster
- Preview and refine Hadoop data (**Hadoop Distributed File System (HDFS)**, Hive, and Impala)
- Run Data Refinery flows on the Hadoop cluster
- Schedule Python or R scripts as jobs to run on Hadoop clusters remotely

The following diagram illustrates distributed processing with Cloud Pak for Data using the Hadoop Integration service to push the processing closer to where the data resides:

![](img/50e1dac0750b69f2b8e8efff000eb13b_136_0.png)

Figure 5.22 – Distributed processing with Hadoop Integration

The Edge Analytics service in Cloud Pak for Data enables the following use cases:

- **Faster insights and actions**: When there is a need for detecting situations and taking action in real time, a centralized analytics approach might not be the most effective as it adds too much network latency for immediate time-critical action to be taken.
- **Continuous operations**: Moving analytics to the edge, close to the device, also enables support for devices that must continue to provide critical functions even if disconnected from the **wide-area network (WAN)**/data center/cloud.
- **Better data and cost control**: With a massive growth in data volumes generated at the edge, outside the traditional data center there is a need to cost-effectively collect and analyze that data. Edge Analytics allows for filtering data at the source, aggregating data at the source where a finer level of detail is not required, and processing sensitive data at the edge.
