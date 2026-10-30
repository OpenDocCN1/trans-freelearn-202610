We can clearly see the empty bucket now.

> **Important note**
> AWS best practices suggest adding another layer of protection through **MFA delete**. Accidental bucket deletions can be prevented, and the security of the objects in the bucket is ensured. MFA delete can be enabled or disabled via the console and CLI. As documented in AWS docs, MFA delete requires two forms of authentication together: your security credentials, and the concatenation of a valid serial number, a space, and the six-digit code displayed on an approved authentication device.

CRR helps you to separate data between different geographical regions. A typical use case can be business-as-usual activities during a disaster. If a region goes down, then another region can support the users if CRR is enabled. This improves the availability of the data. Another use case is to reduce latency if the same data is used by another compute resource, such as EC2 or AWS Lambda being launched in another region. You can also use CRR to copy objects to another AWS account that belongs to a different owner. There are a few important points that are worth noting down for the certification exam:

+   - In order to use CRR, versioning has to be enabled on both the source and destination bucket.
+   - Replication is enabled on the source bucket by adding rules. As the source, either an entire bucket, or a prefix, or tags can be replicated.
+   - Encrypted objects can also be replicated by assigning an appropriate encryption key.
+   - The destination bucket can be in the same account or in another account. You can change the storage type and ownership of the object in the destination bucket.
+   - For CRR, an existing role can be chosen or a new IAM role can be created too.
+   - There can be multiple replication rules on the source bucket, with priority accorded to it. Rules with higher priority override rules with lower priority.
+   - When you add a replication rule, only new versions an object that are created after the rules are enabled get replicated.
+   - If versions are deleted from the source bucket, then they are not deleted from the destination bucket.
+   - When you delete an object from the source bucket, it creates a delete marker in said source bucket. That delete marker is not replicated to the destination bucket by S3.

In the next section, we will cover the concept of securing S3 objects.