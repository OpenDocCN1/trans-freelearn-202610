Now, let’s explore the types of policies that are associated with an IAM role:

+   - **Trust policy**: This policy defines the service that is being trusted. Each role can have only one trust policy. Services that can be trusted include EC2, EMR, an EC2 container service, **Relational Database Service (RDS)**, Lex, and Glue.
+   - **Permissions policy**: The authorization policies specify which resources a role has access to and what actions the role can take on those resources. Policies can be controlled by AWS or the client or added in line with the role itself. More information can be found at https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html.

# Permissions Boundary

This feature allows you to set a boundary or a limit on the maximum permissions that can be assigned through an IAM policy. Permission boundaries allow you to restrict access to perform specific tasks. It is beneficial when there are many employees in an organization, and you want to allow them to create and customize their policies while ensuring that they do not exceed the permissions specified by a permission boundary.

For example, let us say that you have a group of system administrators who should have full access to IAM, S3, and RDS. You want to ensure that they do not launch any EC2 instances. You could add a policy to their account that denies them the ability to launch EC2 instances. However, if they have full access to IAM, they could either remove that restriction or create another IAM user with permission to launch EC2 instances.

A permission boundary can be applied to their account, so they can only create other IAM user accounts if this same permission boundary is attached to those additional accounts. In addition, the permission boundary will explicitly deny the ability to launch EC2 instances. Now, you have a scenario where even if one of the admins tries to create another IAM user account, that new user account will still be denied the ability to launch EC2 instances, preventing any opportunity to circumvent the policies.

In this section, we have reviewed the fundamental concepts of AWS IAM, including its key components, such as the IAM user, IAM roles, and IAM policies.

By default, accessing the web console as an IAM user requires a username and password combination. In addition, you need access keys (comprising access key ID and secret access keys) for command-line access.

Credentials comprising only usernames and passwords are a security risk, which can be mitigated with MFA. In the next section, we will examine this technology in detail.

# Improving Your Security Posture with MFA

MFA is a security feature that enhances the authentication process by requiring users to input a six-digit token in addition to their usernames and passwords. AWS strongly recommends enabling MFA for your root user account and IAM users to increase security.