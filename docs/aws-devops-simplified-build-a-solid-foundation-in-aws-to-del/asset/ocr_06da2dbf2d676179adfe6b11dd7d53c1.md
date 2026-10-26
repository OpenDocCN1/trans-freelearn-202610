# Monitoring

Managing all the application components on their own also meant that extensive monitoring capabilities had to be rolled out, alongside the application itself. Basic CloudWatch monitoring sufficed the need for insights into how the infrastructure components were behaving but was not enough for application observability.

By injecting the CloudWatch agent in the AMIs and creating custom parsers for the application logs, the developers were able to publish relevant metrics onto the monitoring dashboard. Once the metrics were available, it was simple for the developers to derive intelligence from them to dissect system behaviors and configure alerts.

The application had tough latency boundaries to be met by all systems participating in the cluster. This required continuous monitoring and alerting for any threshold breaches. With full control over the AMI, the team was able to inject custom utilities that measured the latency across multiple availability zones and later publish them on CloudWatch.

# Code deployment

Using the Terraform CLI, the developers were able to roll out the entire infrastructure stack using a single `terraform deploy` command. This allowed them to easily provision and manage their applications on multiple AWS regions.

Similar to code deployment practices, the infrastructure stack was also rolled out just like any other software component. AWS CodeDeploy and AWS CodePipeline gave the developers CI/CD capabilities to deploy application components across the entire compute cluster.

# Configuration

IaaS services, unlike PaaS, do not offer application configuration features. They simply don’t manage anything above the network, compute, and storage. This meant that the developers had to design and deploy their own pipelines for rolling out EC2 IAM images for respective AWS regions. The smallest change inside the operating system, or the application configuration, was rolled out in the form of a new AMI revision.

On the other side, this gave a lot of flexibility to the developers in terms of modifying the Linux kernel parameters, software libraries, and disk storage configurations. AWS, as a cloud provider, only provided APIs to manage infrastructure, and the rest was managed by developers with a mix of AWS and open source tools and frameworks.

# Operations

Beanstalk abstracts many details and offers ready-made integrations and best practices by default. This architecture, however, requires everything to be managed by the developers. The journey to manage all application intricacies led to great AWS learnings amongst the developers, which allowed them to automate all the operations they wanted.