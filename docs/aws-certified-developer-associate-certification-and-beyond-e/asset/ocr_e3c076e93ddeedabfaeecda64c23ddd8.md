# AWS Web Management Console

You can access your AWS account using an intuitive web interface. The URL to sign into the web console is https://aws.amazon.com/console/.

Several exercises throughout this book will be completed using the AWS web management console and you will gain hands-on experience in using the console. The following is a screenshot of the AWS Console Home page:

![](bbox=[0.146, 0.252, 0.843, 0.566])

Figure 1.5 – AWS Management Console

To access your AWS account using the web interface, you must either use your root user account or an IAM user account (discussed in the next chapter). The root user owns the AWS account and has full access to all services and resources within the account.

As a best practice, you should not use the root user for day-to-day operations. AWS enables you to create IAM users, which are additional user accounts you can create in your AWS account. These users can represent your colleagues and act on behalf of applications needing access to AWS services. IAM users can be granted access permissions based on their roles, which allows them to interact only with the specific services required for their job responsibilities. This allows you to enforce the principle of least privileges, which may involve assigning administrative permissions to key individuals. This way, you can also ensure that the root user credentials are only used when necessary.

Next, you will explore how you can programmatically access your AWS account using the CLI.