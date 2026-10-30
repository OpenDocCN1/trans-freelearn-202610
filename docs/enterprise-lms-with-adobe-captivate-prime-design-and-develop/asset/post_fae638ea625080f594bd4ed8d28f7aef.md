
## Configuring Login Methods

One thing you may have noticed when creating users in the previous sections is that Captivate Prime does not ask for the user's password upon creation. This is because Captivate Prime does not have any authentication mechanism of its own (except for authenticating external users).

When an internal user tries to log in to Captivate Prime, the system relies on an external authentication mechanism to authenticate that user. By default, the authentication mechanism that's used by Prime's Internal Users is the Adobe ID.

Relying on an external authentication service to authenticate users has several advantages:

- As an organization, you can use the same external authentication service to grant a user access to an unlimited number of applications. This technique is called SSO. The idea is to authenticate once against a central authentication service, which, in turn, gives the user access to all the services for which he/she is authorized. This saves the user from having to authenticate individually to each service.
- When a user changes his/her password, the change is made at the central authentication service. Thus, the new password is immediately available for all applications that the authentication service gives access to. The user does not need to change the password for each service.
- When an employee leaves the organization, deleting the user account at the central authentication service revokes that user's access to all of the organization's applications at once.
- A central authentication service facilitates the daily management of users: updating a user in the central authentication service automatically adjusts his/her access rights to all the organization's tools and applications.

For organizations, SSO has so many benefits that, as an enterprise LMS, Captivate Prime didn't bother developing its own authentication mechanism. Instead, it is designed to integrate seamlessly with the authentication mechanism available in your organization.

If your organization does not have a centralized authentication mechanism, then Adobe provides its own: the Adobe ID. The Adobe ID is nothing more than a central authentication service that gives you access to all the online services provided by Adobe. The Adobe ID you use to access Captivate Prime can also be used to access your Creative Cloud account or online services such as Adobe Spark, Adobe Colors, the Adobe eLearning community, and so on. As such, the Adobe ID is no different from other SSO providers being used by a growing number of organizations around the world.

Because Captivate Prime is designed to use an external SSO provider, it is very easy to configure it to use your organization's SSO provider rather than the Adobe ID. That's what you'll explore in this section. Let's get started:

- 1. Make sure you are logged in as an Administrator.
- 2. From the Administrator home page, click the **Settings** link located in the **Configure** section of the left sidebar.
- 3. Then, click the **Login Methods** link located in the **Integrations** section of the left sidebar. This will take you to the **Login Methods** page, where you can define how internal and external users authenticate to Captivate Prime.

As shown in the following screenshot, the default **Login Method** for internal users is **Adobe ID**, while the default login method for external users is **Captivate Prime ID**:

![](img/5ceaefa2a9a737206c59f0b54e1a07d0_215_0.png)

Figure 5.24 – The default login methods for internal and external users on the Login Methods page

Interestingly, **Captivate Prime ID** is the only authentication mechanism specific to Captivate Prime. It is an exception to the all-external authentication strategy that's used by the platform for internal users.

- 4. Open the **Login** drop-down list under **Internal Users** to inspect the available options.

For internal users, you can choose between **Adobe ID** and **Single Sign-On**. Remember that Adobe ID *is* an SSO mechanism, so the **Single Sign-On** option simply allows you to switch from one SSO provider (Adobe ID) to another (your own)!

If you choose the **Single Sign-On** option, you might receive a message, telling you to contact Adobe Captivate Prime support to enable this feature for your account. The following screenshot has been taken on an account that has the **Single Sign-On** feature enabled:

### Internal Users

![](img/5ceaefa2a9a737206c59f0b54e1a07d0_216_0.png)

Figure 5.25 – Single Sign-On options of an account with SSO enabled

The preceding screenshot shows the options that are available for **Single Sign-On**. Detailing these options is beyond the scope of this book and configuring SSO requires configuration steps both in Captivate Prime and your organization’s IT department. Just be aware that the SSO provider you want to use with Captivate Prime must be compliant with the SAML 2.0 specification. Don’t hesitate to contact the Prime support team for more instructions on how to integrate Captivate Prime with your internal SSO provider.

**SAML 2.0**
The **Security Assertion Markup Language (SAML)** 2.0 standard is an XML-based protocol that uses security tokens to exchange information between a central authentication service and the resource the user wants to access. You can find more information on the SAML 2.0 Wikipedia page at https://en.wikipedia.org/wiki/SAML_2.0.

Now, let’s review the login methods that are available to external users.

- 5. Make sure you revert to the Adobe ID login methods for **Internal Users**.
- 6. Open the **Login** drop-down list for **External Users** to inspect the available options.
- 7. When you’re ready, revert to the **Captivate Prime ID** login method for **External Users**.
- 8. Click the **Cancel** button in the top-right corner of the **Login Methods** page to cancel any modifications you might have made.

For external users, three login methods are available: **Adobe ID**, **Single Sign-On**, and **Captivate Prime ID**, which is the default.

For the examples in this book, make sure you stick to the default login methods, which are **Adobe ID** for **Internal Users** and **Captivate Prime ID** for **External Users**.

In this section, we have discussed **Single Sign-On** and discovered the login methods available in Captivate Prime to authenticate internal and external users. For external users, note that the chosen **Login Method** applies to all users, regardless of the external registration profile that was used when the user registered.

In the next section, you will review how Captivate Prime addresses creating and managing groups of users.

## Using user groups

As the list of users in your Prime account grows, it quickly becomes difficult to manage users individually. This is one of the reasons why administrators generally prefer to manage groups of users.

When creating user groups, you want to group users with common characteristics together. This can be the members of a team, users working at a specific location, users speaking a certain language, users that have the same manager, users that have the same role, and so on. Grouping people in this way has several advantages:

- First, it facilitates users and skills management. For example, you can enroll a group of users in a course rather than enrolling each group member individually. You can also assign skills to user groups rather than assigning skills to each user.
- It also allows certain tasks to be automated. For example, when a user becomes a manager (that is, when he/she becomes a member of the managers' group), he/she is automatically enrolled in the management onboarding course.
- Groups are also useful for reporting purposes. For example, they allow you to generate reports based on the user's location, language, roles, skills, and so on.

In Captivate Prime, each user can be a member of an infinite number of groups, and a group can be a member of another group. In other words, nested grouping is supported in Captivate Prime.

### Inspecting autogenerated groups

Captivate Prime understands that creating groups and manually assigning users to groups is a daunting, time-consuming, and error-prone task. The Captivate Prime team has also noticed that some groups are very common across many organizations. Therefore, a large number of groups are automatically generated by the system based on various aspects, such as roles, user hierarchy, active fields, and more. Let's review these autogenerated groups:

- 1. Make sure you are logged in as an administrator.
- 2. From the Administrator home page, click the Users link located in the Manage section of the left sidebar. This will take you to the Internal Users page.
- 3. From there, click the User Groups link located in the User Groups section of the left sidebar.
