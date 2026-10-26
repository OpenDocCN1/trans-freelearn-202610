![](bbox=[0.12, 0.18, 0.88, 0.56])

# Figure 12.1: Automatic enablement of opt-in AWS regions for new accounts

Not all AWS regions are enabled by default when a new account is created. All AWS regions where you plan to host your workloads need to be opted in first before you can deploy any resources into them. This also means that basic security guardrails, automations, network baselines, and so on cannot work until the new region is configured for use. To handle this use case, one can implement an automated workflow design, as shown in Figure 12.1. The sequential steps for this workflow are described next:

+   1. Platform developers leverage infrastructure as code (IaC) practices to add a new account configuration, using tools such as CloudFormation, Terraform, or Cloud Development Kit (CDK).
+   2. The CI/CD pipeline kicks off and provisions a new account in the AWS Organizations organization.
+   3. The AWS Organizations service emits an event in us-east-1, which is the default region where events from global services are generated.
+   4. To limit access to the organization’s management account, it is a good security practice to forward respective events to a designated automation or tooling account and host all the automation there. In this architecture, we follow the same practice and forward the event to a custom event bus, in another account, in a different region.
+   5. Once the event is received in the target account, an EventBridge rule invokes the AWS Step Functions state machine, which is an AWS service for orchestrating workflows.
+   6. The AWS Step Functions state machine can further trigger several other AWS services and orchestrate their execution. In this example, we use the state machine to invoke two Lambda