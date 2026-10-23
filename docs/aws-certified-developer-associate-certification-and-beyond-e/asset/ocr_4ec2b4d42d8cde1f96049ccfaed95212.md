# Amazon Elastic Beanstalk Components

Amazon Elastic Beanstalk provisions all the resources required for your application. These are application environments within which your application will run. Several components define your Elastic Beanstalk application architecture, which are discussed next.

# Application

An Amazon Elastic Beanstalk application is a collection of components such as environments, versions, and configuration items that can be thought of as a container for your application stack.

# Application Version

This refers to a specific iteration of your application code. Amazon Elastic Beanstalk will store all versions of your application in a managed S3 bucket as deployable code, such as a Java WAR file. A version can be applied to any environment as and when required, and each application version is unique. Application versions can be used for different environments where the latest iteration, for example, may be tested in a development or test environment. Still, your production would host a previous stable version of your application.

# Environment

This is a collection of AWS resources (such as EC2 instances, load balancers, autoscaling groups, and Relational Database Service (RDS) databases) architected to host a specific application version. Each environment can run one application version at a time. For example, you may have three different application versions – a stable production version, a test version, and one being further developed with additional features or bug fixes.

In addition to this, you can have two different environmental tiers:

+   - Web server tier: This is designed to host frontend web applications serving HTTP/HTTPS requests over port 80/443.
+   - Worker tier: This is designed to pull tasks from Amazon Simple Queue Service (Amazon SQS) to process requests accepted by the web server tier.

The following diagram illustrates how a web server tier and worker tier would be architected with Amazon Elastic Beanstalk: