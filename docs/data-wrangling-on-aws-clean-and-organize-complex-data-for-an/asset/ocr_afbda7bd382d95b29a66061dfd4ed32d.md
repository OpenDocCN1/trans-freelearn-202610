![](bbox=[0.13, 0.097, 0.85, 0.627])

Figure 2.13: Amazon S3 data lake folder structure

After the data has been made available on the raw layer of an Amazon S3 bucket, the next step is to create an AWS Glue dataset. In DataBrew, a dataset is comprised of metadata collected about the data. Data can be connected directly or exported to files and loaded from Amazon S3 or a local drive:

To create a DataBrew dataset, follow these steps:

+   1. Go to AWS Glue DataBrew.
+   2. On the left-hand side, click on Datasets and then click on Connect new dataset.