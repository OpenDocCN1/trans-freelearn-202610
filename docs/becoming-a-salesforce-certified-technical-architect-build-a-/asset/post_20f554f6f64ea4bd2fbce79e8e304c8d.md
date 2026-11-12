
## Identity Store

This is a common name given to databases or systems with directory structures that are used to hold information about the users (or, in general, _identities_, which are also known as _principals_) that a particular system is using. Information held in identity stores may include personal details, such as names, phone numbers, and email addresses, in addition to any other information required to authenticate a user, such as the unique user identifier or passwords (remember, passwords should never be stored in plain text or even in an encrypted form that can be decrypted. Passwords are normally stored as hash values that are impossible to reverse/decrypt as you learned previously in _Chapter 3, Core Architectural Concepts: Integration and Cryptography_), and any other data that uniquely describes the user.

The identity store can also contain the user’s _group memberships_, which define the privileges granted to a particular user. The identity store can simply be a database or something more advanced, such as Microsoft **Active Directory** (**AD**).

## Identity Provider

The IDP (or IdP) is a trusted system that manages the identities of known users/principals. It includes capabilities such as creating, updating, and maintaining different identity information. Moreover, it provides authentication services that can be used by other applications. The main difference between modern IDPs and identity stores is that the former could provide identity maintenance and authentication services either by using their own capabilities or relying on connected identity stores. In other words, the IDPs do not necessarily need to maintain the values used for authentication (such as passwords) as they can connect to an identity store to provide the authentication services. The IDPs in this case provide a standardization and orchestration layer of the whole process.

Similarly, the IDP does not need to maintain the personal information of identities as these could be maintained in an identity store. The IDP in this case provides a set of services on top to display, change, and synchronize the data between the applications using the IDP services and the related identity stores. But this does not mean that the IDPs do not have the capability to hold such information natively. Modern IDPs provide the option of either hosting such information or using the services of a connected identity store. Ping Identity, Okta, and other providers are examples of IDPs. Even Salesforce offers limited IDP capabilities.

## Service Provider

The applications that provide specific services for internal or external users are generally referred to as SPs. However, in the context of IAM, SPs are typically applications that rely on other IDPs to provide IAM capabilities (including user management and authentication). SPs typically have their own user repositories (such as databases) and user access control management consoles (where particular permissions are assigned to specific users).

In the use case where an SP is relying on an IDP to handle authentication, identity management, and potentially authorization (commonly known as SSO), there is a need for a special setup where the identity privileges maintained in the IDP would be translated into specific local SP user privileges. In simpler cases, this setup will be limited to the authentication service and a small set of identity management activities.

There are many examples of SPs. Salesforce itself can be considered an SP. Depending on its role in a particular SSO setup, Salesforce can play the role of IDP, SP, or both. Other SP examples include applications such as Workday, SAP, and many others.

## Single Sign-on

The term SSO is used to describe the process where a user is allowed to authenticate once to access multiple applications. A common form of SSO is where a user authenticates to one application using any of the previously mentioned authentication mechanisms (such as credentials) and then attempts to access resources that belong to another application, which is linked with the same SSO mechanism. The user would not need to authenticate again in this case. This can occur either on the user interface (UI) level or the business logic level (typically via APIs). SSO eliminates the need to re-authenticate, which could be a tedious process. Therefore, it is considered a technique to improve user productivity. Moreover, it provides a centralized mechanism to monitor user authentication activities and therefore increases security and compliance.

The SSO setup utilizes an IDP to provide centralized user authentication and policy enforcement capability. For example, it is very common nowadays to see enterprises implement SSO to ensure that all their systems are following the same login policies (such as password complexity requirements, login restrictions, and additional verification mechanisms). These policies and regulations would be enforced by an IDP, which acts as the central unit for the SSO setup. All applications that utilize this IDP would be considered SPs.

As a Salesforce Architect, you need to understand that most, if not all, of the enterprise applications that you might deal with would normally have their own user repositories/databases and have built most of their functionalities based on the assumption that there is a logged-in user (also known as a context user) identified from that local repository. For example, think of applications such as Workday, SAP, Oracle NetSuite, Microsoft Dynamics, and Salesforce. They all expect the user to log in and be identified as a context user or application user against their local data stores.

Therefore, in an SSO setup, users will typically be replicated multiple times across the systems. The IDP will control the full list of users, including their authentication mechanisms (such as their hashed passwords), while each of the SPs would maintain a list of users allowed to work with that application and would rely on the IDP to authenticate those users, so they might not hold any authentication data locally. There will be a need for a unique identifier for each user to map local users to the IDP’s users. This is what is referred to usually as a federated identifier. The federated identifier can take multiple shapes and forms, such as an email address, social security number, or simply a unique enterprise ID.

The following diagram is an example illustrating the global and local data stores in an SSO setup:

![](img/a3b1eecf3c87c814267f2877e8a8be1a_133_0.png)

Figure 4.1 – Example of global and local data stores in an SSO setup

## Multi-Factor Authentication

MFA is used to describe the security mechanism where a successful authentication process would require the user to pass more than one identity validation process using different methods. For example, the user might be requested to provide something they know (such as a password), something they have (such as a one-time token received on a mobile phone), and something they physically possess (such as a fingerprint). In MFA, the user is normally requested to pass three (or more) different security layers. A more relaxed security mechanism is normally referred to as two-factor authentication (2FA), which requires passing just two identity validation processes. A common example (that Salesforce professionals are familiar with) is requesting the user’s credentials at the first step. Upon successfully passing the validation, the user is then redirected to another page with a single input field, and a one-time token is sent to the specified user’s email address. The user would be requested to enter that one-time password (OTP) token into the input field.

Again, this authentication process relies on something the user knows (the password) and something they have (access to their own email inbox). MFA and 2FA ensure that the attacker cannot gain access to the desired resource(s) by simply guessing or stealing a user’s password (or any other single authentication mechanism, such as an ID card). Many enterprises are making this approach mandatory nowadays. Worth mentioning that Salesforce is automatically enabling MFA for direct logins to production orgs in phases, starting with the Spring ’23 release and concluding with Spring ’24.

Having an SSO setup makes this process easy and streamlined. The tasks of defining the 2FA/MFA policies, executing them, and providing a comprehensive log of their use is something the IDP would be handling in full. This means that the SPs do not need to worry about implementing any of this. They can simply rely on the IDP to handle all of that and then authenticate the user who passes the IDP checks based on the trust relationship they have. You will learn more about these trust relationships later in the Understanding the Key Authentication Flows section.

This means that the enterprise can be more agile and flexible in changing and developing its security policies. For example, an enterprise might decide to add a new layer of authentication on top of what it has today. With the right setup, they would need to introduce this only once to the IDP that manages the entire SSO processes for the enterprise.

## User Provisioning and De-provisioning

Provisioning is the process of creating a user in an identity repository. That includes global and local identity repositories. Normally, there will also be a set of roles assigned to the user in each repository such as the permission to read some database tables. De-provisioning is exactly the opposite process and involves deactivating or deleting an identity from global and local identity stores, including any roles associated with it, terminating any open sessions, and revoking existing tokens.
