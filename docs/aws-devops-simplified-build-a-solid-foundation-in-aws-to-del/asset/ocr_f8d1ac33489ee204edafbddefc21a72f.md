# Test strategies for your AWS workloads

Code is no longer restricted to the business application that your end users consume. Your infrastructure, CI/CD pipelines, and all the automations that you build to support your application’s delivery are equally good candidates for risking your security posture. Assuming most (if not all) components of your architecture are deployed via pipelines, there are two main aspects to consider:

+   - Security of the pipeline
+   - Security in the pipeline

Let’s take a look.

# Security of the pipeline

A high degree of automation built into your pipelines also means that any unintended code changes can go very far in your delivery life cycle, eventually reaching production. Good security practices restrict any form of human access in productive environments, which, on the other hand, requires CI/CD pipelines to have highly privileged access permissions. Therefore, it is important to ensure that your pipelines are also treated as code, continuously checked, and monitored for security breaches. Five main areas that require safeguarding will be discussed next.

# Identity and Access Management for your pipelines

AWS Identity and Access Management (IAM) helps manage the identities and their access to AWS services, for both human and non-human access patterns. For example, the CodePipeline pipelines we deployed in previous chapters used a service role, where we define what the service can or cannot do in the AWS account on the user’s behalf. Depending on the actions a pipeline needs to carry out, you should always aim to give minimum permissions based on the principle of least privilege.

Companies with highly mature DevSecOps practices can also consider building pipeline factories, which can emit the least privileged reusable templates for pipelines targeting common application patterns.

# Detecting unusual behaviors

You should capture all the execution logs of your pipelines in AWS, just like you would do for an application. Furthermore, services such as CodePipeline and CodeBuild offer log and metric integration capabilities with CloudWatch Logs and CloudWatch Metrics. Enabling these settings is a matter of adding a few configuration lines in your infrastructure code.

Maintaining the confidentiality and integrity of these logs is an important security measure and a good organizational strategy additionally captures all the IAM action events in CloudTrail. This facilitates seamless audit procedures when needed. For example, a sudden surge in denial of IAM access requests could hint at compromised systems and can be tracked using the Error code