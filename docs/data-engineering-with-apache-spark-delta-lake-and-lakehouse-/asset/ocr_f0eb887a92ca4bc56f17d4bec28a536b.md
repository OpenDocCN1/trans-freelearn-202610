Here are some case scenarios where you might use a Lambda architecture with great success:

+   - **Access to near-real-time data**—There are scenarios in which the decision-making process is detrimentally impacted if access to the latest data is not available.
+   - **Large historical datasets**—There are situations in which the historical data is extremely large, so aggregations can take a long time to finish and cannot be performed frequently.
+   - **Dynamic nature of analytics (ad hoc)**—There are cases in which the case scenarios for data analytics are very dynamic and vary quite frequently. In this case, the batch and real-time views can be created or adjusted without affecting the underlying master data.

> **Important note**
> A particular strength of the Lambda architecture is its strength to be easily migrated from traditional architecture. I have done several of these migrations in the past with great success.

Lambda architecture provides a nice balance of performance and data freshness. But in recent times, requirements for streaming analytics have grown quite sharply. Use cases such as IoT monitoring and anomaly detection require a fast real-time analytics engine able to support millions of incoming requests per second.

Despite all the advantages that Lambda architecture has, there are a few drawbacks, as outlined here:

+   - In a few cases, it is sometimes complex to implement due to the multiple layers. More layers also translate to more costs, in resources, administration, and monitoring.
+   - Each layer requires a different set of tools, services, and programming. Therefore, infrastructure and code management become a challenge.
+   - In some scenarios, implementing a Lambda architecture may be overkill due to the nature of the analytics.

Quite recently, a simpler architecture was introduced, the Kappa architecture, which many call the simplified version of the Lambda architecture.