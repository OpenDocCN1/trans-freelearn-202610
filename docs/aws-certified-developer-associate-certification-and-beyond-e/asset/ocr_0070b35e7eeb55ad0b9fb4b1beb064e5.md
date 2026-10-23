For example, you can create a stack that builds a shared VPC and creates resources such as a public subnet and a security group. The subnet ID and the security group ID can then be exported as outputs of that stack. Next, you can build another stack for deploying EC2 instances in the same VPC that will be used to host your application. In the second stack, you would import the VPC ID, subnet ID, and the security group ID required to ensure that the EC2 instance is deployed in the correct subnet and attached to the relevant security group. The following diagram illustrates this concept.

![](bbox=[0.13, 0.23, 0.85, 0.51])

When using cross-stack templates, you would use `Fn::ImportValue` to import the required values into the second stack being built.

Ultimately, the `Outputs` section enables you to get easy access to the details of the resources created and used often with cross-stack CloudFormation configurations.

In the next section, you will learn more about a security feature known as CloudFormation capabilities. This comes into play when creating specific resources using CloudFormation, where you must acknowledge that certain types of resources will be created as a security measure.

# # CloudFormation Capabilities

Often, you may need to create specific resources, but even if you have permission to do so, you must still *acknowledge* to CloudFormation that you are creating such resources. This is more for security and assurance that you are aware of the types of resources being created by the template.