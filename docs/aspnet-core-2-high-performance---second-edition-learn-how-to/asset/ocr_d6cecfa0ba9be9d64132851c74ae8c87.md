You will learn how to identify and fix issues that are not always apparent when everything is running on a single machine. You'll see how to identify when your O/RM or framework behaves badly and is too chatty with the database, which can easily happen if it's not used correctly.

We will see how to ensure that work is performed in the most appropriate place, and we'll look at some ways of keeping your images small using the correct resolution and format. These techniques will ensure that your application is efficient and that data is not sent over the wire unnecessarily.

We'll also discuss how to mitigate performance issues with an alternative approach by improving the underlying hardware to reduce the factors that amplify issues in bad software. This can be a good temporary measure if the software application is already deployed to production and is in use. If you already have live performance problems, then this can buy you some time to engineer a proper fix.

# Latency

As covered in the previous chapters, latency is the delay that occurs before an operation can complete, sometimes also known as lag. You may not be able to control the latency of the infrastructure that your software application runs on, but you can write your application in such a way that it can cope with this latency in a graceful manner.

The two main types of latency that we will discuss here are network latency and disk latency. As the names suggest, these are, respectively, the delay in performing an operation over the network and the delay in reading from or writing to a persistent storage medium. You will often deal with both at the same time, for example, a database (DB) query to a server on a remote virtual machine will require the following operations:

+   - A network operation from the web server to the DB server
+   - A network operation from the DB server to a remote disk on a Storage Area Network (SAN)
+   - A disk operation to look up data on the physical drive

![](bbox=[0.12, 0.75, 0.18, 0.83])

Although Solid State Drives (SSDs) have much lower latency than spinning platter disks, they are still relatively slow. When we talk about disk I/O here, we refer to both types of drive.