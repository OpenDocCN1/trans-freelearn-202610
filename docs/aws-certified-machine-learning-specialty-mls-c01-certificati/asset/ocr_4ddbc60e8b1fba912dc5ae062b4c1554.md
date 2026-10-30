+   - Relational Database Services (RDSes)
+   - Managing Failover in Amazon RDS
+   - Taking automatic backup, RDS snapshots, and restore and read replicas
+   - Writing to Amazon Aurora with multi-master capabilities
+   - Storing columnar data on Amazon Redshift
+   - Amazon DynamoDB for NoSQL databases as a service

# Technical requirements

All you will need for this chapter is an AWS account and the AWS CLI configured. The steps to configure the AWS CLI for your account are explained in detail by Amazon here: https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html.

You can download the code examples from Github, here: https://github.com/PacktPublishing/AWS-Certified-Machine-Learning-Specialty-MLS-C01-Certification-Guide/tree/master/Chapter-5/.

# Storing data on Amazon S3

S3 is Amazon's cloud-based object storage service and it can be accessed from anywhere via the internet. It is an ideal storage option for large datasets. It is region-based, as your data is stored in a particular region until you move the data to a different region. Your data will never leave that region until it is configured. In a particular region, data is replicated in the availability zones of that region; this makes S3 regionally resilient. If any of the availability zones fail in a region, then other availability zones will serve your requests. It can be accessed via the AWS Console UI, AWS CLI, or AWS API requests, or via standard HTTP methods.

S3 has two main components: **buckets** and **objects**.

+   - Buckets are created in a specific AWS region. Buckets can contain objects, but cannot contain other buckets.
+   - Objects have two main attributes. One is **Key**, and the other is **Value**. Value is the content being stored, and Key is the name. The maximum size of an object can be 5 TB. As per the Amazon S3 documentation available here, https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingObjects.html, objects also have a version ID, metadata, access control information, and subresources.