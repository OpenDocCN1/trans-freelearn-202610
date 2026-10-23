Ultimately, you might use either user pools, identity pools, or even both depending on your requirements. In summary, user pools are designed to restrict access to APIs to authorized users for web and mobile applications – you can create and manage your own pool for managing your users’ identities (or federate with third-party providers). You can also create groups and associate your users as appropriate. Once your users are authenticated, Amazon Cognito issues access tokens, which are then used to grant access to APIs via API Gateway or an Application Load Balancer.

If you need to provide access directly to backend services such as Amazon S3 or DynamoDB, then you would need to use identity pools because you will need to exchange your access tokens for AWS temporary credentials. These AWS temporary credentials are created and managed by the Amazon STS, which we discuss next.

# How AWS STS Generates Temporary Tokens

The STS service will automatically generate credentials that grant limited access to AWS resources depending on permissions defined in policy documents attached to the identity. Both IAM users and federated users can be assigned temporary credentials.

Some differences exist between temporary security credentials and permanent access keys for IAM users. While they function similarly, temporary security credentials have some unique features:

+   - Temporary credentials are created for a specified duration and expire after a specific time
+   - You can configure credentials expiration from a few minutes to several hours
+   - AWS does not recognize the credentials after expiration, and any API requests made after expiry will be denied unless permissions to renew the credentials continue

By default, calls to AWS STS are made to its global endpoint at https://sts.amazonaws.com, which redirects the request to the N. Virginia (us-east-1) Region. However, you should consider using regional endpoints, most of which are enabled by default to ensure low latency, add redundancy, and increase session token validity.