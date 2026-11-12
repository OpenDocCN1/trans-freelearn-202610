# # SSO

The option for SSO functionality can also be provided with a hybrid identity. This allows users already signed in to a device with their AD credentials to be signed in to cloud resources and services automatically.

SSO works with federated authentication, such as AD FS, and cloud authentication, such as PTA. This ensures that any requests for access to Azure AD-authenticated resources are validated by AD as the IDP and not Azure AD. It is more secure in its network operation as it does not require inbound access; all traffic is initiated as outbound from the sync agents and is secured through Microsoft-managed certificates.

PTA provides a similar capability for SSO as AD FS, although it is less complex to implement and operate as it requires no AD FS infrastructure.

The term SSO should not be confused with the same sign-on, which can also confusingly be referred to as SSO.

Directory synchronization provided through the cloud authentication method of PHS allows users to access cloud resources using the same credentials. Still, they will be required to enter them again when accessing Azure AD-authenticated resources when already signed into AD.

This section looked at implementing SSO. In the next section, we will complete a hands-on exercise section to re-enforce some of the concepts covered in this chapter.

# # Hands-on exercise

To support your learning with some practical skills, we will utilize the concepts and understanding gained from the previous sections of this chapter and practically apply them.

We will look at the following exercise:

+   - Exercise – Implementing Azure AD Connect

# # Getting started

To get started with this section, if you do not already have an Azure subscription, you can create a free Azure account at https : //Azure.microsoft.com/free. This free Azure account provides the following:

+   - 12 months of free services

+   - $200 credit to explore Azure for 30 days

+   - Over 25 services that are always free

Let's move on to the exercise for this chapter.