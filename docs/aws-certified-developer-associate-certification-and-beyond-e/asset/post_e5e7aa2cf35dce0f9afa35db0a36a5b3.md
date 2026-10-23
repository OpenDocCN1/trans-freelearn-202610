
## Security Token-Based MFA

When MFA is enabled, a trusted source must provide an authentication code to access the Management Console. MFA can be set up for API calls to AWS and user console logins using a hardware- or software-based (virtual) MFA device.

AWS offers the **Universal 2nd Factor (U2F)** as an MFA option based on an open authentication standard. U2F can use specialized USB keys or **near-field communication (NFC)** devices such as smart card technology, but AWS only supports USB-based U2F and not NFC devices for MFA.

For more details on U2F, feel free to review the information at https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_u2f.html.

Users must have an IAM user account to use MFA tokens, which use a one-time pin approach based on time synchronization and six-digit numeric digits for identification. MFA provides an additional layer of protection, requiring an MFA token, a username, and a password to access an account. An IAM user's MFA token is generated only by a synchronized MFA device and changes periodically.

## Accessing AWS Accounts Using Identity Federation with SAML 2.0 and Microsoft AD

Microsoft AD is the most used IAM service across companies of all sizes, especially with reference to on-premises workloads. Microsoft AD stores information about users, roles, and permissions across an organization and provides an alternative authentication approach for the AWS platform. In the next section, you will learn how to link your AWS accounts to an existing **AD Federation Service (AD FS)**.

In this section, we discuss another option that leverages an existing IdP: Microsoft AD and SAML 2.0 for authentication with your AWS account. SAML 2.0 is an open standard for exchanging authentication and authorization tokens between an IdP such as Microsoft AD FS and a **service provider (SP)**, in this case, AWS. SAML 2.0 is often used to facilitate **single-sign-on (SSO)** capabilities, which enables users to access multiple services with a single set of credentials without having to log in separately to each service.

Many businesses use an AD environment to manage authentication and authorization for on-premises-hosted services. In large organizations with thousands to millions of users, their credentials, permission policies, and access across multiple platforms can become increasingly difficult to manage. Identity federation enables you to grant permissions to external identities to use the AWS Management Console or APIs, eliminating the need to create new IAM user accounts for existing users and thus reducing the management overhead. By using AD FS, you can seamlessly connect your organization's AD resources to AWS.

The next diagram depicts how you configure AD FS services to allow federated access to users in your AD environment:

![](img/34ff9b5b5a734dae3450ab701e223c73_80_0.png)

Figure 2.4 – Workflow showing the authentication process using an AD

The following is a breakdown of how you can access the Management Console using AD FS, as shown in Figure 2.4:

- 1. In an internet browser, you request an AD FS website. https://HostName/adfs/ls/IdpInitiatedSignOn.aspx is an example of this URL. When you enable the AD FS service on a Windows-based server, it generates a virtual directory on your webserver called adfs. The login page is generated automatically.
- 2. After receiving the credentials, AD FS verifies the user’s identity against Windows AD.
- 3. The AD FS provides a SAML assertion to the user browser to facilitate an authentication response.
- 4. The SAML assertion is posted to the AWS endpoint by the user’s browser. Under the hood, AssumeRoleWithSAML will request a temporary token, and then a sign-in link to log on to the AWS web interface.
- 5. The user can then use the sign-in URL and is redirected to the Management Console.

Microsoft AD is great for securing internal resources, but what about the external applications/ resources? In the next section, we will discover how you can use Amazon Cognito and third-party IdPs to secure your workloads.

## Using Web Identity Federation for Application Authentication

Most web and mobile applications require some form of authentication and authorization at the application layer before an end user can use its services.

In the case of the new Todo List application, customers will need to sign up for a subscription. Next, to ensure that only valid customers can access this application, a sign-in process will need to be built as part of the authentication and authorization security feature of the application.

Developing a database of users and verifying their credentials is a crucial step in any application’s development life cycle. However, storing credentials data within the application code or on devices is highly insecure and risky. To address this issue, Amazon recommends using web identity federation for web and mobile application authentication workflows. This allows applications to request temporary security credentials dynamically from an IdP, and an IAM role can generate the credentials with a token provider.

Web identity federation enables sign-up and sign-in with a wide variety of trusted third-party IdPs such as Amazon, Google, and other OIDC-compliant IdPs, eliminating the need to create usernames and passwords for logging in. The software issues a token once a user is verified, granting temporary AWS access. Note that these credentials are linked to a specific IAM role and can only be used for tasks explicitly authorized by that role.

To avoid the security risk of embedding or distributing credentials with applications, Amazon Cognito is strongly recommended to be used with web-identity federation rather than developing a custom solution. Amazon Cognito simplifies mobile and web application registration and login processes, supporting authentication of third-party IdPs.

The blog post at `https://aws.amazon.com/blogs/aws/aws-iam-now-supports-amazon-facebook-and-google-identity-federation/` provides an excellent example of employing online identity federation.

In the next section, we look at how Amazon Cognito can help you streamline your application authentication and authorization workflow.

## Application Authentication and Authorization with Amazon Cognito

Amazon Cognito is an identity service for your web and mobile applications. The service allows you to create a user directory or federate with external IdPs such as Google, Amazon, and Facebook to name a few. The internal directory (or external IdP) is used to verify a user’s identity and authorize their access to your application. This service removes the overhead associated with trying to create your own custom identity service for your application.

Amazon Cognito comprises two key components, which can either be used independently or in tandem. We discuss these next.

### Amazon Cognito User Pools

Amazon Cognito user pool is a user directory service that offers user creation, management, and authentication services for your applications. End users can sign up for a user account, which results in creating a user profile. You can create an independent user directory hosted in Amazon Cognito and use OIDC-generated **JSON Web Tokens (JWTs)** for authentication and authorization.

Alternatively, you can also use third-party IdPs such as Facebook, Google, and Amazon, and once the user has been authenticated, exchange those third-party tokens for Cognito ones. This allows you to avoid creating yet another directory, resulting in users having to remember yet another set of credentials.

Amazon Cognito user pool allows you to build sign-up and sign-in workflows and has the added feature of storing and managing user data. You can also use groups to manage your users more effectively.

Additional key features include the following:

- The ability to use the hosted **user interface (UI)** for sign-up and sign-in or create custom UIs
- Authenticate with username and password; sign-ups need to be verified, for example, by email
- Implement MFA for enhanced security
- OAuth 2.0 flows
- Integration with a third-party IdP such as Facebook, Google, or Amazon
- Integrate Lambda triggers to add hooks in the login flow for added functionality
- You can also migrate from third-party existing pools into Cognito user pools

Once a user logs in via a Cognito user pool setup, access tokens such as a JWT are issued, which can be used to secure access to some AWS services.

#### Access Backend Resources with a User Pool

With user pools, once your user has signed in, your web or mobile app will receive user pool tokens that can be used to grant access to backend resources, as per the following diagram. For example, if a user needs to update their profile information on a social media platform, the data is ultimately stored in a backend database. For the user's update to be successful, the user must first be authenticated and authorized, which is handled in this case by Cognito.
