
However, you may wish to retain certain resources even after deleting a stack because you need to store data generated or collected as part of a project for compliance or backup requirements. For example, you may wish to retain an S3 bucket that was used to store data uploaded to it, even after deleting a stack that created the bucket. In such cases, you can configure a deletion policy to retain the S3 bucket after the deletion of the stack. An example CloudFormation snippet illustrates this configuration in the following code snippet:

```
AWSTemplateFormatVersion: '2010-09-09'
  Resources:
    myS3Bucket:
      Type: AWS::S3::Bucket
      DeletionPolicy: Retain
```

The following are the four deletion policy options you can configure:

- **Delete**: This is the default configuration when you delete a stack and you do not need to specify DeletionPolicy. However, you should note that for certain resources, the default policy is set to Snapshot, such as AWS::RDS::DBCluster and AWS::RDS::DBInstance.
- **Retain**: CloudFormation will retain resources instead of deleting them when the stack is deleted. You would need to add a deletion policy to the resource type in the template.
- **RetainExceptOnCreate**: This is similar to Retain, except that if the stack operation created the resource and then needs to roll back, CloudFormation deletes the resource. In a stack delete operation, CloudFormation will retain the resource.
- **Snapshot**: This is for those resources that support snapshots. CloudFormation will create a snapshot before deleting the resource. Examples of this are RDS DB instances and RDS DB clusters.

> **Important Note**
> For Amazon S3 buckets, a deletion policy set to delete will only work if the bucket is empty. Often, you might find that the stack fails to delete your S3 buckets because they are not empty, and you must, therefore, either manually empty the bucket or configure a custom resource, discussed later in this chapter.

Sometimes, when CloudFormation doesn’t have the resources you require in its available resource types, you may need to create custom resources. This can also include provisioning custom logic in your CloudFormation templates. In the next section, you will learn more about custom resources.

# CloudFormation Custom Resources

Custom resources can be used when you need to define a resource that currently isn’t available as a resource type in CloudFormation. Custom resources can also be used when you need to define custom logic for how your CloudFormation stack is to be created, updated, or deleted.

To define a custom resource, you use the AWS::CloudFormation::CustomResource or Custom::MyCustomResourceTypeName resource types in your templates.

For example, if you need to delete an Amazon S3 bucket as part of a stack delete request, you must first ensure the S3 bucket is empty. If the S3 bucket has any objects contained within it, the delete request will fail. In this case, you can define a custom resource to first call a Lambda function that will empty the S3 bucket so that you can perform the bucket delete operation.

When constructing the custom resource, you would then define a property, namely a service token specifying where CloudFormation is to send the request to, for example, the Lambda function ARN. Note that the service token must be in the same Region where you create the stack. The following code snippet is an illustration of this.

```
Resources:
  MyCustomResources:
    Type: AWS::CloudFormation::CustomResource
    Properties:
      ServiceToken: String
```

Often, you may need the logic within your template to ensure that certain resources are created before others. In this example, you must ensure that the S3 bucket is emptied before it is deleted. Here, you can use the DependsOn attribute. When you add a DependsOn attribute to a resource, that resource is created only after the creation of the resource specified in the DependsOn attribute. So, in our example, the deletion of the S3 bucket will depend on the custom resource that calls the Lambda function to empty the S3 bucket first.

In the next section, you will learn about another feature of CloudFormation, called nested stacks.

# CloudFormation Nested Stacks

Nested stacks are extremely useful for quickly deploying infrastructure and application components across multiple accounts and Regions. Furthermore, they allow you to ensure that a centrally defined set of templates is used to describe and provision resources in other locations.

When building infrastructure resources on AWS, you may repeatedly use common components that form part of your overall architecture. For example, when deploying EC2 instances for an application solution, you first need to provision a VPC, some subnets, and a security group to associate with your instance.

An important note here is that for every CloudFormation stack you create, you are limited to 500 resources per stack. In highly complex stacks, you may find this limitation an issue. Furthermore, resources created by one stack may not be usable in another stack. For example, in the case of our real-world example with Todo Plus Limited, you would want different CloudFormation stacks for your various environments, such as development, testing, and production. You wouldn’t want to reuse the VPC components created in the development environment for your production workloads. Ultimately, the lifecycle of each stack will vary, and you will most definitely want to isolate your environments from each other.

Rather than repeatedly adding the configuration for your common components, such as VPCs, subnets, and security groups, you can create dedicated templates for those components and reference those templates from your main CloudFormation template. For example, rather than including the configuration for your VPC in the main application stack template, you can create a separate template that defines your VPC configuration and reference it from your main application stack. This allows you to reuse the same VPC templates across multiple stacks you create for any number of solutions. These are known as nested stacks. Nested stacks can also contain other nested stacks, which means you can build a hierarchy of stacks. The following diagram illustrates the nested stack concept.

![](img/34ff9b5b5a734dae3450ab701e223c73_575_0.png)

Figure 13.8: Nested stack example

The nested stack can be created using the AWS::CloudFormation::Stack resource. The only property required for this resource is TemplateURL, which is how CloudFormation knows how to find the template for the resource.

When building a nested stack, the root stack is the top-level stack, which contains all nested stacks. This root stack is also known as the parent stack. Child stacks can have further nested stacks for which they become a parent stack and so on.

![](img/34ff9b5b5a734dae3450ab701e223c73_577_0.png)

A key component to facilitate the use of StackSets is using IAM roles to ensure that necessary permissions exist to deploy stacks in target accounts and Regions. You can create StackSets using either self-managed permissions or service-managed permissions:

- Self-managed permissions: You create and configure necessary IAM roles to establish trust relationships between the administration and target accounts.
- Service-managed permissions: You deploy stack instances to accounts managed by AWS Organizations. This prevents you from creating and managing your own IAM roles manually and makes it easy to deploy stacks in the accounts you create in the future.

An essential component of StackSets is the stack instance. This refers to a stack in a target account, allowing StackSets to deploy stacks in the target account. Stack instances contain information about the stack in the target account. They can exist without a stack if a stack in a target account cannot be created. The stack instance would show the reason for the failure in this case.

In the next section, you will learn more about CloudFormation stack roles. Remember that, on AWS, no action is permitted unless you explicitly grant permissions, and CloudFormation is no exception to this.
