
- Relational Database Services (RDSes)
- Managing Failover in Amazon RDS
- Taking automatic backup, RDS snapshots, and restore and read replicas
- Writing to Amazon Aurora with multi-master capabilities
- Storing columnar data on Amazon Redshift
- Amazon DynamoDB for NoSQL databases as a service

## Technical requirements

All you will need for this chapter is an AWS account and the AWS CLI configured. The steps to configure the AWS CLI for your account are explained in detail by Amazon here: https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html.

You can download the code examples from Github, here: https://github.com/PacktPublishing/AWS-Certified-Machine-Learning-Specialty-MLS-C01-Certification-Guide/tree/master/Chapter-5/.

## Storing data on Amazon S3

S3 is Amazon's cloud-based object storage service and it can be accessed from anywhere via the internet. It is an ideal storage option for large datasets. It is region-based, as your data is stored in a particular region until you move the data to a different region. Your data will never leave that region until it is configured. In a particular region, data is replicated in the availability zones of that region; this makes S3 regionally resilient. If any of the availability zones fail in a region, then other availability zones will serve your requests. It can be accessed via the AWS Console UI, AWS CLI, or AWS API requests, or via standard HTTP methods.

S3 has two main components: **buckets** and **objects**.

- Buckets are created in a specific AWS region. Buckets can contain objects, but cannot contain other buckets.
- Objects have two main attributes. One is **Key**, and the other is **Value**. Value is the content being stored, and Key is the name. The maximum size of an object can be 5 TB. As per the Amazon S3 documentation available here, https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingObjects.html, objects also have a version ID, metadata, access control information, and subresources.

**Important note**
As per Amazon's docs, S3 provides read-after-write consistency for PUTS of new objects, which means that if you put a new object or create a new object and you immediately turn around to read the object using its key, then you get the exact data that you just uploaded. However, for overwrites and deletes, it behaves in an **eventually consistent manner**. This means that if you read an object straight after the delete or overwrite operation, then you may read an old copy or a stale version of the object. It takes some time to replicate the content of the object across three availability zones.

A folder structure can be maintained logically by using a prefix. Let's take an example where an image is uploaded into a bucket, `bucket-name-example`, with the prefix `folder-name` and the object name as `my-image.jpg`. The entire structure looks like this: `/bucket-name-example/folder-name/my-image.jpg`.

The content of the object can be read by using the bucket name as `bucket-name-example` and the key as `/folder-name/my-image.jpg`.

There are several storage classes offered by Amazon for the objects being stored in S3:

- **Standard Storage**: This is the storage class for frequently accessed objects and for quick access. S3 Standard has a millisecond first byte latency and objects can be made publicly available.
- **Reduced Redundancy (RR)**: This option provides less redundancy than the Standard Storage class. Non-critical and reproducible data can be stored in this class. AWS S3 documentation suggests not to use this class as the Standard Storage class is more cost-effective.
- **Standard Infrequent Access (IA)**: This option is used when you need data to be returned quickly, but not for frequent access. The object size has to be a minimum of 128 KB. The minimum storage timeframe is 30 days. If the object is deleted before 30 days, you are still charged for 30 days. **Standard IA** objects are resilient to the loss of availability zones.
- **One Zone Infrequent Access**: The object of this storage class is stored in just one availability zone, which makes it cheaper than **Standard IA**. The minimum object size and storage timeframe are the same as **Standard IA**. Objects from this storage class are less available and less resilient. This storage class is used when you have another copy, or if the data can be recreated. A **One Zone IA** storage class should be used for long-lived data that is non-critical and replaceable, and where access is infrequent.
- **Glacier:** This option is used for long-term archiving and backup. It can take anything from minutes to hours to retrieve objects in this storage class. The minimum storage timeframe is 90 days. You cannot use the Amazon Glacier API to access the objects moved from S3 to Glacier as part of object life cycle management.
- **Glacier Deep Archive:** The minimum storage duration of this class is 180 days. This is the least expensive storage class and has a default retrieval time of 12 hours.
- **Intelligent Tiering:** This storage class is designed to reduce operational overheads. Users pay a monitoring fee and AWS selects a storage class between Standard (a frequent access tier) and Standard IA (a lower cost infrequent access tier) based on the access pattern of an object. This option is designed for long-lived data with unknown or unpredictable access patterns.

Through sets of rules, the transition between storage classes and deletion of the objects can be managed easily, and are referred to as **S3 Lifecycle Configurations**. These rules consist of actions. These can be applied to a bucket or a group of objects in that bucket defined by prefixes or tags. Actions can either be **Transition actions** or **Expiration actions**. Transition actions define the storage class transition of the objects following the creation of a *user-defined* number of days. Expiration actions configure the deletion of versioned objects, or the deletion of delete markers or incomplete multipart uploads. This is very useful for managing costs.

An illustration is given in Figure 5.1. You can find more details available here: https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html:

![](img/209e007237d34506e7ba16180f2e905c_164_0.png)

Figure 5.1 – A comparison table of S3 Storage classes

## Creating buckets to hold data

Now, let's see how to create a bucket, upload an object, and read the object using the AWS CLI.

1. In the first step, we will check whether we have any buckets created by using the `aws s3 ls` command:
```
$ pwd
/Users/baba/AWS-Certified-Machine-Learning-Specialty-2020-Certification-Guide/Chapter-5/s3demo/demo-files
$ aws s3 ls
```
2. This command returns nothing here. So, we will create a bucket now by using the `mb` argument. Let's say the bucket name is `demo-bucket-baba` in the `us-east-1` region:
```
$ aws s3 mb s3://demo-bucket-baba --region us-east-1
make_bucket: demo-bucket-baba
$ aws s3 ls
2020-11-04 14:39:50 demo-bucket-baba
```
3. As we have created a bucket now, our next step is to copy a file to our bucket using the `cp` argument, as shown in the following code:
```
$ aws s3 cp sample-file.txt s3://demo-bucket-baba/
upload: ./sample-file.txt to s3://demo-bucket-baba/sample-file.txt
```
4. To validate the file upload operation via the AWS console, please log in to your AWS account and go to the AWS S3 console to see the same. The AWS S3 console lists the result as shown in Figure 5.2. The console may have changed by the time you're reading this book!

![](img/209e007237d34506e7ba16180f2e905c_166_0.png)

Figure 5.2 – AWS S3 screenshot to list your files

You can also list the files in your S3 bucket from the command line, as shown here:

```
$ aws s3 ls s3://demo-bucket-baba/
2020-11-04 14:50:02        99 sample-file.txt
```

- If there is a scenario for uploading your filesystem directories and files to the S3 bucket, then --recursive will do the job for you:

```
$ aws s3 cp . s3://demo-bucket-baba/ --recursive
upload: folder-1/a.txt to s3://demo-bucket-baba/folder-1/a.txt
upload: folder-2/sample-image.jpg to s3://demo-bucket-baba/folder-2/sample-image.jpg
upload: ./sample-file.txt to s3://demo-bucket-baba/sample-file.txt
$ aws s3 ls s3://demo-bucket-baba/
```
