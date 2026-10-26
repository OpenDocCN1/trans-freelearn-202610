
Figure 11.3 – Orchestration of different AWS services by AWS Control Tower

This diagram is not an extensive representation of every single function that AWS Control Tower takes care of under the hood. Rather, it aims to highlight the different steps involved in an account creation workflow, followed by deploying resources into it. Let’s take a look at this diagram in more detail:

- 1. The platform developer uses the AWS Control Tower console UI to register a new account creation request, as well as the target OU the account should be part of.
- 2. AWS Control Tower passes on the request to Account Factory (a Service Catalog product), which initiates the account provisioning workflow.
- 3. Under the hood, the request results in the AWS Organizations API being invoked to create a new account.
- 4. An AWS account gets created and placed in the target OU, which defines what kind of policy controls and governance will apply to the workloads hosted in this account.
- 5. AWS CloudFormation StackSets, a service feature that automatically deploys stacks into an account-region combination, already has some reusable templates defined by the AWS Control Tower service. A new stack instance for the StackSet definition is created by AWS Control Tower, which initiates the provisioning of an AWS CloudFormation template in the target account, in
- 6. AWS CloudFormation stack provisioning starts inside the target account, in a particular AWS region, and deploys resources such as the AWS Config recorder, IAM roles, and other things that are needed to manage this account in a compliant manner.
- 7. AWS Config sets up a *recorder* that monitors changes to all resources in the accounts and sets up a delivery channel
- 8. The configuration recorder is a mechanism that registers any resource configuration changes and sends the data to a centralized S3 bucket for auditing needs; notifications are sent to an SNS topic in parallel. The SNS notifications are aggregated per region and can be used for advanced analysis and automated alerts by the security teams when unexpected resource changes happen.
- 9. When you first install AWS Control Tower, it creates an organization-wide AWS CloudTrail trail that automatically picks up new accounts in the organization and aggregates trail data into the central S3 bucket in the LogArchive account. It also pushes logs to the CloudWatch log group in the management account. This provides audit and investigation possibilities to a central security team without the need to access all the accounts in the Landing Zone.

All of these features are available out of the box with Control Tower, which takes roughly 1 hour to install. As you might have noticed, there is no application or organization-specific deployment need that the service covers. So far, it’s a prescriptive setup of good practices that will be rolled out as soon as an account is created. To roll out other AWS services and resources, you would need a deployment solution or framework that fills the gaps needed to get to a complete Landing Zone platform.

Next, let’s have a look at CfCT. This is an AWS-supported solution that integrates with the AWS Control Tower service to roll out custom resources defined by AWS CloudFormation templates. Say you require a particular IAM role for your security scanning third-party platform that should be automatically deployed in all AWS accounts as soon as they are provisioned. CfCT can help with this. Alternatively, you might want to deploy certain Config rules into all AWS accounts contained in the production OU, but not others.

## Deploying resources with CfCT

CfCT is a deployment framework that integrates with your Control Tower service deployment. It needs to be installed in the management account, in the same region where Control Tower was initially set up. It hooks onto the service events and runs a workflow that provisions SCP policies or AWS CloudFormation resources across account targets in your organization.

The framework is used to deploy two types of resources: SCP policies and CloudFormation StackSets. Each has a dedicated deployment workflow that is implemented through AWS Step Functions. User interaction with the framework happens through a manifest file that has a well-defined YAML format. Whenever the file is committed to version control, the framework picks up the changes and executes the pipeline.

We can look at **Figure 11.4** to understand the different components involved in the deployment framework:

![](img/ecd265dfca9bb64161408ba8a944a4d9_276_0.png)

Figure 11.4 – Components of the CfCT framework (source: https://docs.aws.amazon.com)

The entire solution can be deployed as a CloudFormation template in your AWS management account. As you can see, there is a CodePipeline pipeline that pulls the most recent changes from the version control (S3 or CodeCommit) and triggers a CodeBuild project that validates the manifest file initially. If all looks good, the framework proceeds with an invocation for both the SCP Step Functions state machine and the CloudFormation Step Functions state machine. If you haven’t been exposed to the AWS Step Functions service before, it is a workflow orchestrator that helps you build programmable flows that invoke various AWS services to get a job done. Let’s look at an example of the manifest file that’s used by the framework to deploy resources in your Landing Zone:

```yaml
---
region: us-east-1  # Control Tower Home Region
version: 2021-03-15
resources:
  - name: test-preventive-guardrails
    description: Prevent deleting or disabling resources
    resource_file: s3://marketplace-sa-resources-ct-us-east-1/ctlabs/preventive-guardrails.json
    deploy_method: scp
    deployment_targets:
      organizational_units:
        - Security
        - Sandbox
...
- name: create-iam-role
  resource_file: s3://marketplace-sa-resources-ct-us-east-1/ctlabs/describe-regions-iam-role.template
  deploy_method: stack_set
  deployment_targets:
    organizational_units:
      - Security
      - Sandbox
    regions:
      - us-east-1
```

The manifest file answers three questions, represented by keys in the YAML structure:

- `resource_file`: Where is the code that needs to be deployed?
- `deploy_method`: What needs to be deployed? This can have two values – `scp` or `stack_set`.
- `deployment_targets`: Where should the resource be deployed? Here, you can use individual accounts or OUs that represent a collection of accounts.

As soon as this file is committed into the Git repository tracked by the solution, the pipeline triggers, resulting in the SCP and the CloudFormation template being deployed in the target destinations – in this case, the security and sandbox OUs.

One benefit of using AWS-offered solutions is that you are covered by AWS Support, should the need arise. This is not the case with other custom implementations that you might develop.

## Summary

We started this chapter by identifying the need for a multi-account structure and the benefits it provides when hosting multiple workloads in AWS. Multiple services, solutions, and deployment frameworks fulfill a certain need, but it’s important to understand that they are not going to help you just on their own. Some level of customization is always needed for organizations to implement their specific needs. With the needs and benefits clarified, we moved on to key considerations that you should have when designing a Landing Zone concept. We also looked at some best practices in the areas of multi-account structure, security and IAM, DevOps and Config management, and operations.

Toward the second half of this chapter, we took a deeper look into how AWS Control Tower works under the hood, and out-of-the-box integrations with the CfCT solution. One of the key benefits that customers get from these solutions is that they are supported by AWS, just as they would support any other service.

In the next chapter, *Adhering to AWS Well-Architected Principles*, we will investigate the key pillars of a solid foundation that you can consider for your application workloads.
