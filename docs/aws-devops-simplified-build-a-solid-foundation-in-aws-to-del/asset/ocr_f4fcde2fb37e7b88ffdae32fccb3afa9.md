# Ensuring a Strong AWS
Foundation for Multi-Account
and Multi-Region Environments

An AWS account is a *logical container* for the AWS resources that you create. It provides a structure that can be used to easily manage the access, billing, and isolation needs of a group of related resources.

All organizations start their AWS journey by creating an account and hosting some **proof of concept (PoC)** workloads on it. As they gain more experience and know-how, their confidence and comfort with cloud services increases. At a certain point, the organizations might decide on migrating their on-premises production workloads to the cloud. Or, even better, they might directly start developing cloud-native applications by leveraging AWS services for maximized innovation.

There is often a pivoting point where they must decide if all their workloads should be hosted in one or multiple AWS accounts. Key drivers behind these discussions are the problems associated with a single account. These problems surface when the AWS platform’s usage outgrows its initial intent of hosting PoC workloads and transitions into a full-grown environment that now requires enterprise-grade security, governance controls, cost management, granular access policies, and so on.

However, the nature of these problems could vary across different organizations operating at different scales. Some might find it difficult to manage granular security policies for multiple users in a single account, while others might struggle with boundary enforcement between development and production workloads, for example. In addition to the application challenges, there are also service limits and quotas that are set by the cloud provider, which could limit the usage of a single AWS account beyond certain thresholds. Whatever the reason for the initial conversations, all organizations face these challenges once their AWS usage matures and will have to adopt a multi-account strategy.