# Project Task 13.2: Delete a CloudFormation Stack

The following steps will help you delete a CloudFormation stack:

+   1. While still logged in to your AWS Production account as Bob, navigate to the CloudFormation console.
+   2. From the left-hand menu, click **Stacks**.
+   3. In the right-hand pane, select the stack you just created.
+   4. Click the **Delete** button in the top right-hand corner of the **Stacks** pane.
+   5. In the dialog box that appears, confirm by clicking on the **Delete** button.
+   6. Your CloudFormation stack will now be deleted, removing the VPC and its various resources. You can review the deletion process by navigating to the **Events** tab and reviewing the deletion events.
+   7. In the following section, you’ll find a summary of the key points from this chapter.

# Summary

This chapter covered two critical AWS services for managing infrastructure as code. CloudFormation allows you to define and deploy AWS resources using YAML or JSON templates. The AWS CDK is a software development framework for defining cloud infrastructure in code and provisioning it through AWS CloudFormation.

This chapter explained the main components of CloudFormation and the AWS CDK and provided a sample CloudFormation template that creates a VPC with subnets, gateways, route tables, and security groups.

The chapter also highlighted some best practices for using CloudFormation, such as defining and testing templates before deployment, creating change sets, and strategies for varying configuration options such as StackSets and nested stacks, and discussed the CloudFormation template anatomy in detail.

CloudFormation and the CDK can help you create repeatable and reproducible applications and infrastructure solutions in minutes. You avoid configuration drift and ensure your environments are built as expected. In addition, you can enforce governance and security in your deployments.