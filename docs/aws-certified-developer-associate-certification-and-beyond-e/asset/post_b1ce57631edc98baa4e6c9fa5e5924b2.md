
![](img/34ff9b5b5a734dae3450ab701e223c73_83_0.png)

Figure 2.5 – Access your backend resources with a user pool

Another option is to access your applications and APIs hosted on AWS via API Gateway or an Application Load Balancer. Specifically, you would use your tokens to protect an API and implement authorization for its access. This can be done using a Cognito authorizer (of the COGNITO_USER_POOLS type), to grant access to your application via API Gateway or an Application Load Balancer.

Once the API has been deployed, the authenticated user can retrieve an identity or access token and use this to invoke the API method with the tokens attached to the authorization header.

The following diagram illustrates this workflow:

![](img/34ff9b5b5a734dae3450ab701e223c73_83_1.png)

Figure 2.6 – Access resources with API Gateway and Lambda with a user pool

Amazon Cognito user pools are designed to be used for restricting API access for web and mobile applications.

However, user pools cannot be used to directly access other AWS services such as Amazon S3 or Kinesis for example. For this requirement, you will need to create Amazon Cognito **identity pools**. Let’s discuss that next.

## Amazon Cognito Identity Pools

Amazon Cognito identity pools are designed to be used when you want to authorize authenticated or even anonymous access directly to your resources. With identity pools, you either use identities from a Cognito user pool or a third-party IdP and exchange a user pool token (or tokens by the third-party IdP) for temporary AWS credentials. You then use these credentials to access AWS APIs and the AWS CLI.

When your app assigns the credentials sessions to an authenticated user, you can grant access to backend AWS resources such as Amazon S3, Kinesis, and DynamoDB.

Another feature of Cognito identity pools is the ability to grant access to guest users (anonymous access, as mentioned earlier). This involves granting users a unique identifier and AWS credentials even if they haven’t authenticated through an IdP. To enable guest access you will need to activate it in the user’s tab of the identity pool.

You can also combine the usage of both Amazon Cognito user pools and identity pools, as per the following diagram, to facilitate the sign-up and sign-in of your users, management of their profiles, access to your application, and even access to backend AWS services and resources. If you do not need direct access to the backend resources, then you do not require those temporary AWS credentials, in which case, Amazon Cognito user pools should suffice for your use case.

![](img/34ff9b5b5a734dae3450ab701e223c73_84_0.png)

Figure 2.7 – Access AWS services with a user pool and an identity pool

Ultimately, you might use either user pools, identity pools, or even both depending on your requirements. In summary, user pools are designed to restrict access to APIs to authorized users for web and mobile applications – you can create and manage your own pool for managing your users’ identities (or federate with third-party providers). You can also create groups and associate your users as appropriate. Once your users are authenticated, Amazon Cognito issues access tokens, which are then used to grant access to APIs via API Gateway or an Application Load Balancer.

If you need to provide access directly to backend services such as Amazon S3 or DynamoDB, then you would need to use identity pools because you will need to exchange your access tokens for AWS temporary credentials. These AWS temporary credentials are created and managed by the Amazon STS, which we discuss next.

## How AWS STS Generates Temporary Tokens

The STS service will automatically generate credentials that grant limited access to AWS resources depending on permissions defined in policy documents attached to the identity. Both IAM users and federated users can be assigned temporary credentials.

Some differences exist between temporary security credentials and permanent access keys for IAM users. While they function similarly, temporary security credentials have some unique features:

- Temporary credentials are created for a specified duration and expire after a specific time
- You can configure credentials expiration from a few minutes to several hours
- AWS does not recognize the credentials after expiration, and any API requests made after expiry will be denied unless permissions to renew the credentials continue

By default, calls to AWS STS are made to its global endpoint at https://sts.amazonaws.com, which redirects the request to the N. Virginia (us-east-1) Region. However, you should consider using regional endpoints, most of which are enabled by default to ensure low latency, add redundancy, and increase session token validity.

The following are common actions you can perform with the AWS STS service:

- **AssumeRole**: This returns temporary credentials that can be used to access a resource in your AWS account or to perform cross-account access. The credentials will consist of an access key ID, a secret access key, and a security token. In the IAM roles section discussed earlier, you learned that if the Todo List frontend website were to be hosted on EC2 instances and the website required access to media content hosted on an S3 bucket, an appropriate role would need to be assigned to the EC2 instances as an instance profile. Here, the IAM role would consist of two policies: a trust policy that defines who or what can assume the role, which in this case is the EC2 service. The other policy comprises the set of permissions that the entity that assumed the role can perform on the resource. So, in this case, it would be the ability to retrieve the media content hosted in S3 using the GET request. The following screenshot illustrates this AssumeRole trust policy:

![](img/34ff9b5b5a734dae3450ab701e223c73_86_0.png)

Figure 2.8 – The AWS STS AssumeRole trust policy

- **AssumeRoleWithSAML**: This returns temporary credentials assigned to authenticated users via a SAML authentication process. Often used for corporate identity federation to enable SSO capability, organizations can federate authentication via their default IdP. This allows users to log in using their existing IdP authentication credentials and enables role-based access to AWS. Temporary credentials generated using AssumeRoleWithSAML last 1 hour by default but can be configured to a value between 1 hour to 12 hours.

- **AssumeRoleWithWebIdentity**: This returns temporary credentials for users authenticated in a web or mobile application with a web IdP such as an OAuth 2.0 provider such as Amazon, Google, or Facebook. This enables you to build and deploy applications that request temporary credentials instead of having to store long-term credentials in the application. This is made possible because the users are authenticated using a token from the web IdP.

- GetSessionToken: This returns temporary credentials for an IAM user and is typically used when you want to protect certain AWS API operations such as EC2 StopInstances with MFA for additional security. The IAM user will utilize the GetSessionToken function and provide the MFA code from their MFA device, enabling them to execute instance stop requests.

In this section, you learned about AWS STS and how it facilitates the creation and management of temporary security credentials used by features such as IAM roles.

Next, let us look at a high-level overview of the Todo List application design architecture and specifically highlight the Amazon Cognito service and how it is integrated into the design.

## Amazon Cognito and the Todo List Application

Our client, TodoPlus Limited, will utilize the Amazon Cognito service to facilitate end-user sign-up and sign-in processes for their Todo List application. For the purposes of this application, you will also be using a custom UI instead of the hosted UI. Amazon Cognito will be integrating with several other AWS services to offer security features. These include protection from distributed denial of service (DDoS) attacks and integration with API Gateway to control who calls these API operations.

In the following diagram, you will note the overall application architecture for the Todo List application. Don't worry if the architectural workflow seems a bit unfamiliar at this point. As you progress through this study guide, you will learn about each of the core fundamental technologies that will be used to design, build, and deploy this application on AWS. For now, we highlight some of the key AWS services we will be using at a high level. We then deep dive into each of the services in subsequent chapters.

![](img/34ff9b5b5a734dae3450ab701e223c73_87_0.png)

Figure 2.9 – The Todo List application’s high-level architectural design
