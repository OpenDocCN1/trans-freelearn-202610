In addition, Azure AD has different integrations that can handle identity federation, which also allows using it for authentication.

From a life cycle perspective, consider the following:

+   - Look at the use of integrating Azure AD with third-party applications from identity federation, using a protocol such as SAML or OAuth. This means that the service will use Azure AD as the point for authentication, allowing us to use security mechanisms in Azure AD in addition to allowing us to use SSO once the user has been authenticated. If you have services that do not support Azure AD natively, Microsoft also has password-based SSO integration, which uses a browser extension that is inserted into the logon session of the user.

+   - If you are using cloud-based HR systems, such as **Workday** or **SuccessFactors**, you can use the built-in HR provisioning service in Azure AD to automatically provision users created in those HR systems.

+   - Look at the use of **SCIM**-based provisioning for applications/services that support this. This allows Azure AD to automatically manage the life cycles of users in those services. For legacy systems that do not support SCIM, Microsoft also has custom provisioners for systems that use either **SQL** or **LDAP**-based authentication protocols. Some third-party services also support direct integration with Azure AD using APIs.

While this list is not extensive by any means, Microsoft has fairly good documentation on how to set up these different integrations:

+   - SCIM-based provisioning: https://docs.microsoft.com/en-us/azure/active-directory/app-provisioning/on-premises-scim-provisioning

+   - Azure AD and identity federation using SAML: https://docs.microsoft.com/en-us/azure/active-directory/saas-apps/tutorial-list

In addition to this, Azure AD also supports other authentication protocols for authentication (https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/auth-sync-overview), which provides a good foundation for the identity pillar under the zero-trust architecture.

# Ensuring strong passwords and authentication methods

Many organizations today have not fully implemented MFA in their services, meaning that users can access external services without any form of secondary authentication, only a username and password.

Fortunately, more organizations are implementing this. Microsoft stated in a blog post in 2019 that by implementing MFA, you can block 99.9% of all account compromises: https://www.microsoft.com/security/blog/2019/08/20/one-simple-action-you-can-take-to-prevent-99-9-percent-of-account-attacks/.