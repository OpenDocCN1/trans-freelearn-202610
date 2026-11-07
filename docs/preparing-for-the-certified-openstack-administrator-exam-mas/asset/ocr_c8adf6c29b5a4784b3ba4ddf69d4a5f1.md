# Exam objectives

Let's take a look at the Swift exam objectives. Remember that the Certified OpenStack Administrator exam expects you to understand how to use OpenStack from the perspective of an administrator, not an infrastructure or DevOps engineer. You do not need to be overly concerned with the backend Swift configuration. This is a complex topic that you can learn more about in the book OpenStack Object Storage (Swift) Essentials by Packt Publishing.

The following figure breaks down various Swift-related objectives you should be responsible for knowing on the COA exam. As you can see, only some of these tasks can be completed on both the Horizon dashboard and CLI. When it comes to ACLs and object expiration, you will need to master the CLI commands shown in this chapter.

![](bbox=[0.22, 0.4, 0.81, 0.52])

Figure 8.3: Swift ACLs and object expiration can only be completed via the CLI

To upload objects to Swift on the Horizon dashboard, the files must be accessible via your local filesystem. If you want to upload files larger than 5 GB, you must use the CLI.

# Exam objective - managing containers and objects

Containers are analogous to buckets in the Amazon S3 world. Containers are owned by the project (also known as an account in the Swift world). When it comes to permissions, Swift containers act a bit differently from traditional OpenStack resources. By default, a user must have the admin or swiftoperator role in order to create, delete, and modify containers. We will learn about how to set ACLs in the upcoming section.