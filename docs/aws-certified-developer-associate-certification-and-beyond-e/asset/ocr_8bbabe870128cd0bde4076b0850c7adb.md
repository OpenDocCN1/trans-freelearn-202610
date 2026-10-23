Once the preceding configuration is in place, the developers can switch roles and access the TodoApp S3 bucket in AWS Account B.

**Note**
With the new IAM Identity Center service (previously known as AWS Single Sign-On), you may not need to use cross-account access extensively for IAM users. This is because the IAM Identity Center offers an alternative approach to granting your workforce users access to multiple out-of-the-box accounts in your AWS Organizations. These users can be from AWS IdPs as well as external providers. We discuss the IAM Identity Center later in this chapter.

+   - **Federation:** Applications can use identity federation to authenticate external users using external IdPs without needing a custom sign-in code. This can be achieved using Amazon, Facebook, Google, or other IdPs that are compatible with **OpenID Connect (OIDC)**, Microsoft AD, or Protocol (LDAP), which support SAML 2.0. By doing so, token-based authentication strategies can be developed between external IdPs and applications hosted on AWS, eliminating the need for separate user accounts and passwords.

# IAM Policy

A JSON document adheres to the IAM policy standard, which outlines the permissions defined by a specific IAM role. An example of such a policy is depicted here, which allows the ability to access objects stored in the TodoApp S3 bucket:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AccessToDoAppBucket",
            "Action": [
                "s3:GetObject",
                "s3:ListBucket"
            ],
            "Effect": "Allow",
            "Resource": [
                "arn:aws:s3:::todoapp",
                "arn:aws:s3:::todoapp/*"
            ]
        }
    ]
}
```