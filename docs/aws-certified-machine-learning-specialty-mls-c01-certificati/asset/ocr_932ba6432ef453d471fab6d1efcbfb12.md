11. Which two of the following are correct?

+   - A. Multi-AZ:Same Region::Read Replica:Multiple Region
+   - B. Multi-AZ:Multiple Region::Read Replica:Same Region
+   - C. Multi-AZ:Synchronous Replication::Read Replica:Asynchronous Replication
+   - D. Multi-AZ:ASynchronous Replication::Read Replica:Synchronous Replication

12. Which of the following is correct?

+   - A. Read replicas are read-only instances.
+   - B. Read replicas are read-write instances.
+   - C. Read replicas are write-only instances.
+   - D. Read replicas are read-only instances, until promoted to read-write.

13. Where is EFS accessible from?

+   - A. Inside a VPC only
+   - B. Via AWS endpoints
+   - C. Anywhere with a public internet connection
+   - D. Inside a VPC or any on-premises locations connected to that VPC via a hybrid network

14. Which three of the following are true?

+   - A. Instance store volumes are persistent storage.
+   - B. Instance store volumes are temporary (ephemeral) storage.
+   - C. Data stored on instance store volumes can be lost if a hardware failure occurs.
+   - D. Data stored on instance store volumes can be lost when an EC2 instance restarts.
+   - E. Data stored on instance store volumes can be lost when an EC2 instance stops and starts.

> **Answer**
> Hardware failure can change the underlying host. So, there is no guarantee of the instance store volume. When you stop the instance and start it again, the instance store volume is lost due to the change of host. Instance restarting is different from stop and start; it means an operating system restart.