Amazon Cognito comprises two key components, which can either be used independently or in tandem. We discuss these next.

# Amazon Cognito User Pools

Amazon Cognito user pool is a user directory service that offers user creation, management, and authentication services for your applications. End users can sign up for a user account, which results in creating a user profile. You can create an independent user directory hosted in Amazon Cognito and use OIDC-generated **JSON Web Tokens (JWTs)** for authentication and authorization.

Alternatively, you can also use third-party IdPs such as Facebook, Google, and Amazon, and once the user has been authenticated, exchange those third-party tokens for Cognito ones. This allows you to avoid creating yet another directory, resulting in users having to remember yet another set of credentials.

Amazon Cognito user pool allows you to build sign-up and sign-in workflows and has the added feature of storing and managing user data. You can also use groups to manage your users more effectively.

Additional key features include the following:

+   - The ability to use the hosted **user interface (UI)** for sign-up and sign-in or create custom UIs
+   - Authenticate with username and password; sign-ups need to be verified, for example, by email
+   - Implement MFA for enhanced security
+   - OAuth 2.0 flows
+   - Integration with a third-party IdP such as Facebook, Google, or Amazon
+   - Integrate Lambda triggers to add hooks in the login flow for added functionality
+   - You can also migrate from third-party existing pools into Cognito user pools

Once a user logs in via a Cognito user pool setup, access tokens such as a JWT are issued, which can be used to secure access to some AWS services.

# *Access Backend Resources with a User Pool*

With user pools, once your user has signed in, your web or mobile app will receive user pool tokens that can be used to grant access to backend resources, as per the following diagram. For example, if a user needs to update their profile information on a social media platform, the data is ultimately stored in a backend database. For the user's update to be successful, the user must first be authenticated and authorized, which is handled in this case by Cognito.