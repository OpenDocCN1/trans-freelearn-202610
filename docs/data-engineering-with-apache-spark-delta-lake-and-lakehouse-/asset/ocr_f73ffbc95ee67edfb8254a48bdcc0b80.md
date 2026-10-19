+   - Understanding data consumption
+   - Verifying aggregated data in the gold layer
+   - Meeting customer expectations

# The need to aggregate data

Before we deep dive into how to aggregate data and build the gold layer of the Electroniz lakehouse, we should quickly remind ourselves why we’re building the lakehouse in the first place. During the discovery phase, Electroniz had put forth some key business requirements, as follows:

+   - They want to aim for a near-real-time sales analytics platform that can serve their customer needs faster and better.
+   - They want to streamline their advertising budgets through intelligent analytics based on the number of user hits on their e-commerce website.
+   - They want to decrease customer dissatisfaction because of interruptions in fulfilling orders due to inventory shortages.

Thus far, we have been able to identify, collect, and curate data from a variety of sources to help us perform intelligent decision-making using modern analytics. At this point, we have data in the silver layer that represents the state of the data that has been standardized, validated, de-duplicated, and secured. But does it provide the answers to all the business requirements that Electroniz wanted us to address? In short, the answer is not yet. So, how do we go about looking for the right answers?

> **Important**
> In data terms, the silver layer of every dataset is referred to as an "observation." Each dataset has multiple rows that are referred to as "variables."

The objective of creating the gold layer is to **aggregate** or **summarize** data. **Aggregation** is a technique that’s used to identify patterns over a collection of observations using a set of variables. Some common examples of aggregations are the total sales over a given quarter, the total number of users per month who visited your website, and the average salary of your employees per department.

Largely, the structure (schema) of the data in the silver layer is very similar to the source where it was ingested from. On the contrary, the structure of aggregated data is either two-dimensional like a table or multi-dimensional like a cube: