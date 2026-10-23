The following screenshot demonstrates AWS requesting such an acknowledgment.

![](bbox=[0.15, 0.14, 0.85, 0.26])

Figure 13.3: AWS CloudFormation capabilities

Here, you will note the resources require the `AWS::IAM::ManagedPolicy` capability and the user is being prompted to acknowledge this.

When deploying CloudFormation templates using the CLI, you can use the `--capabilities` option to acknowledge the fact that the stack will be creating resources that affect permissions in your AWS account such as the creation of **AWS Identity and Access Management (IAM)** users or roles.

There are three capabilities options that you can specify:

+   - `CAPABILITY_IAM`: Required if your stack needs will affect IAM resources, such as the creation of an IAM user.
+   - `CAPABILITY_NAMED_IAM`: Required if you provide custom names for the IAM resources being created.
+   - `CAPABILITY_AUTO_EXPAND`: Required if you use template macros such as the `AWS::Serverless` transform macro, which converts **Serverless Application Module (SAM)** templates into CloudFormation templates.

In the next section, you will learn about a CloudFormation feature called change sets. Often, you may need to update or change the resources in an existing stack. This feature enables you to visualize your changes before implementing them and can be used as part of your overall change management process.

# CloudFormation Change Sets

Often, you may need to update and change the infrastructure you previously deployed using a CloudFormation template. You can create **change sets** and request that CloudFormation updates the stack. You can provide a modified template or input parameter values to submit changes to your stack.