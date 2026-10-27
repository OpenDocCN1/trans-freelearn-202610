# Permissions

Service role info
Amazon Lookout for Metrics needs permission to read data from your bucket. Choose a service role with access to your data, or create a role automatically.

Enter the ARN of a role

Custom service role ARN
arn:aws:iam::...:role/LookoutMetricsAccessRole

Figure 14.18 – Datasource details – Permission definition

> Note
> As an alternative, you can also directly create a suitable role at this stage by selecting Create a role in the Service role dropdown. Amazon Lookout for Metrics will ask you to specify which Amazon S3 bucket you would like to give some permissions for and a role will be created on your behalf.

6. Scroll down to the bottom of the screen and click on Next. Amazon Lookout for Metrics tries to access your data and validate your dataset. Once validation is complete, you get a success dialog box where you can click on OK to continue.

7. On the next page, you will map the different fields of your dataset to the Measures and Dimensions of Amazon Lookout for Metrics. Let's start by adding two measures (you can have up to five of them). Your detector will track these fields of your CSV file for anomalies. You will map the views and revenue columns of your CSV file with Amazon Lookout for Metrics measures. For each of these fields, you will also request the service to perform a SUM aggregation across all dimensions.

# Measures Info

Measures are the key performance indicators (KPIs) that your detector tracks for anomalies.

Create measures using fields from your datasource. The detector will track these measures for anomalies. You can have up to 5 measures.

Field name
views
revenue
Aggregate by
SUM
SUM
Add measure
You can add up to 3 more measures.

Figure 14.19 – Defining the measures in your dataset