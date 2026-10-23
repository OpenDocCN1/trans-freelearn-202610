Once the dataset has been reviewed, click on **Create dataset**.

![](bbox=[0.148, 0.138, 0.87, 0.245])

Figure 2.17: Dataset in the AWS Glue DataBrew console

The dataset will now be ready, and data discovery can be performed.

# Data discovery – creating an AWS Glue DataBrew profile for a dataset

To create an AWS Glue DataBrew data profile for a dataset, follow these steps:

+   1. Choose a job name, for example, `raw-human-resource-profile-job`.
+   2. Select **Full Dataset** for the job run sample.
+   3. Choose the S3 location where you want to store the profile output in JSON format, which can be used for automating tasks such as data quality checks and PII identification.
+   4. Keep the rest of the options as the defaults.
+   5. Under **Permissions**, choose **Create new IAM role** and provide a name for the role, such as `human-resource`.
+   6. Click on **Create and run job**.
+   7. Wait for the data profile to be generated.

After the data profile is created, you can review it in the summary section, which provides useful information about the dataset, including the following:

+   - The size of the dataset, which serves as a good data quality check to ensure no data loss has occurred during ingestion
+   - The data type of the dataset, including the number of integer, double, and string columns
+   - Any duplicate rows or missing cells, which can help identify data quality issues