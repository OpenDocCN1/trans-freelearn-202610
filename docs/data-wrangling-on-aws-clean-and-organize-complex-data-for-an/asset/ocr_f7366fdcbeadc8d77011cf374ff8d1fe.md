# # Options available for data wrangling on AWS

Depending on customer needs, data sources, and team expertise, AWS provides multiple options for data wrangling. In this section, we will cover the most common options that are available with AWS.

# AWS Glue DataBrew

Released in 2020, AWS Glue DataBrew is a visual data preparation tool that makes it easy for you to clean and normalize data so that you can prepare it for analytics and machine learning. The visual UI provided by this service allows data analysts with no coding or scripting experience to accomplish all aspects of data wrangling. It comes with a rich set of common pre-built data transformation actions that can simplify these data wrangling activities. Similar to any Software as a service (SaaS) (https://en.wikipedia.org/wiki/Software_as_a_service), customers can start using the web UI without the need to provision any servers and only need to pay for the resources they use.

# SageMaker Data Wrangler

Similar to AWS Glue DataBrew, AWS also provides SageMaker Data Wrangler, a web UI-based data wrangling service catered more toward data scientists. If the primary use case is around building a machine learning pipeline, SageMaker Data Wrangler should be the preference. It integrates directly with SageMaker Studio, where data that's been prepared using SageMaker Data Wrangler can be fed into a data pipeline to build, train, and deploy machine learning models. It comes with pre-configured data transformations to impute missing data with means or medians, one-hot encoding, and time series-specific transformers that are required for preparing data for machine learning.

# AWS SDK for pandas

For customers with a strong data integration team with coding and scripting experience, AWS SDK for pandas (https://github.com/aws/aws-sdk-pandas) is a great option. Built on top of other open source projects, it offers abstracted functions for executing typical data wrangling tasks such as loading/unloading data from various databases, data warehouses, and object data stores such as Amazon S3. AWS SDK for pandas simplifies integration with common AWS services such as Athena, Glue, Redshift, Timestream, OpenSearch, Neptune, DynamoDB, and S3. It also supports common databases such as MySQL and SQL Server.

# Summary

In this chapter, we learned about the basics of data wrangling, why it is important, the steps and best practices of data wrangling, and how the data wrangling steps vary based on persona. We also talked about the different data wrangling options available in AWS.

In the upcoming chapters, we will dive deep into each of these options and learn how to use these services to perform data wrangling.