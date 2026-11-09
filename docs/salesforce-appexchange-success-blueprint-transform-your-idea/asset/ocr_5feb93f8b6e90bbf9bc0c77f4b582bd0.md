If you decide to include a connected app in your package, one recommended way to configure a connected app for secure integration is to use the OAuth 2.0 JWT Bearer Flow. This robust and secure flow allows the application to authenticate itself to Salesforce, without the need to share sensitive information such as passwords or security tokens.

# How (and why) your customer can install your connected app

Installing a connected app is more than just facilitating integration between Salesforce and external platforms. For your customer’s system administrators, it’s about gaining control and enhancing security. By installing and configuring a connected app, administrators can define who can use the apps, where they can access them from, and under what conditions. This encompasses setting up profiles, permission sets, IP range restrictions, and even implementing multi-factor authentication (MFA) for bolstered security.

Before diving into the installation process, it’s crucial to educate your customer’s system administrator on the importance and methodology of installing the connected app. This step ensures that they understand the significance of the app and can assist or take over the installation if necessary.

There are several methods for installing a connected app on your customer’s Salesforce org:

+   - Through a Managed Package: This is one of the most straightforward methods, here, the system administrator installs a managed package that inherently contains the connected app.