We can look at **Figure 11.4** to understand the different components involved in the deployment framework:

![](bbox=[0.136, 0.126, 0.754, 0.435])

Figure 11.4 – Components of the CfCT framework (source: https://docs.aws.amazon.com)

The entire solution can be deployed as a CloudFormation template in your AWS management account. As you can see, there is a CodePipeline pipeline that pulls the most recent changes from the version control (S3 or CodeCommit) and triggers a CodeBuild project that validates the manifest file initially. If all looks good, the framework proceeds with an invocation for both the SCP Step Functions state machine and the CloudFormation Step Functions state machine. If you haven’t been exposed to the AWS Step Functions service before, it is a workflow orchestrator that helps you build programmable flows that invoke various AWS services to get a job done. Let’s look at an example of the manifest file that’s used by the framework to deploy resources in your Landing Zone:

```
```yaml
---
region: us-east-1  # Control Tower Home Region
version: 2021-03-15
resources:
  - name: test-preventive-guardrails
    description: Prevent deleting or disabling resources
    resource_file: s3://marketplace-sa-resources-ct-us-east-1/ctlabs/
preventive-guardrails.json
    deploy_method: scp
    deployment_targets:
      organizational_units:
        - Security
        - Sandbox
...
```
```