# Figure 11.3 – Orchestration of different AWS services by AWS Control Tower

This diagram is not an extensive representation of every single function that AWS Control Tower takes care of under the hood. Rather, it aims to highlight the different steps involved in an account creation workflow, followed by deploying resources into it. Let’s take a look at this diagram in more detail:

+   1. The platform developer uses the AWS Control Tower console UI to register a new account creation request, as well as the target OU the account should be part of.
+   2. AWS Control Tower passes on the request to Account Factory (a Service Catalog product), which initiates the account provisioning workflow.
+   3. Under the hood, the request results in the AWS Organizations API being invoked to create a new account.
+   4. An AWS account gets created and placed in the target OU, which defines what kind of policy controls and governance will apply to the workloads hosted in this account.
+   5. AWS CloudFormation StackSets, a service feature that automatically deploys stacks into an account-region combination, already has some reusable templates defined by the AWS Control Tower service. A new stack instance for the StackSet definition is created by AWS Control Tower, which initiates the provisioning of an AWS CloudFormation template in the target account, in