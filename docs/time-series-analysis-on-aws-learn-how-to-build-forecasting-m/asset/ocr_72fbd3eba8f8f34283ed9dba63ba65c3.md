# Pricing

As with many AWS services, you only pay for what you use with no upfront commitment. Although the cost of the service is minimal to build a proof of concept (especially if you can benefit from the Free Tier), there is only one dimension to consider to operationalize an anomaly detection pipeline with Amazon Lookout for Metrics: the number of metrics a detector analyzes during a given month. You will be billed irrespective of the frequency at which your metrics are analyzed: whether your metric issues data once per hour or once per day, you will still be billed for a single metric.

If this is the first time you have used Amazon Lookout for Metrics with any given account, you have access to the Free Tier, which allows you to use the service for free for one month. During this period, there will be no charge for analyzing up to 100 metrics.

AWS service developers work relentlessly at reducing the operational costs of the services and price reductions happen regularly. At the time this book was written, your Amazon Lookout for Metrics usage is billed according to the following tiered pricing:

+   - First 1,000 metrics: $0.75 per metric
+   - From 1,000 to 5,000 metrics: $0.50 per metric
+   - From 5,000 to 20,000 metrics: $0.25 per metric
+   - From 20,000 to 50,000 metrics: $0.10 per metric
+   - Every metric beyond the 50,000th one will be billed at $0.05.

For the most up-to-date pricing, you can check the Amazon Lookout for Metrics pricing page at https://aws.amazon.com/lookout-for-metrics/pricing/.

Note that depending on how you use Amazon Lookout for Metrics, other charges may occur. This will happen, for instance, if you do any of the following:

+   - Create alerts using Amazon SNS.
+   - Create custom actions with AWS Lambda functions.
+   - Use AWS AppFlow to import data into a Lookout for Metrics dataset.

For more details about the pricing models of these services, check out their respective pricing pages:

+   - Amazon SNS: https://aws.amazon.com/sns/pricing/
+   - AWS Lambda: https://aws.amazon.com/lambda/pricing/
+   - AWS AppFlow: https://aws.amazon.com/appflow/pricing/