One of the key checks the security review process described in the previous chapter makes is to scan the code to ensure that it is using the appropriate Apex conventions to enforce the security rules administrators of your application configure, as not all security checks are enforced automatically for you.

This chapter discusses the following two categories of security as provided by the platform:

+   - Functional security: Security that applies to application objects and some code entry points (providing APIs) and also Visualforce pages that deliver the features of the application is referred to here as functional security
+   - Data security: In contrast to security applied to individual record data created and maintained by the application, this type of security is referred to as data security

# Functional security

Both profiles and Permission Sets can be created and packaged in your application to help administrators of your application control access to your objects, fields, tabs, and pages. Fundamentally, profiles have been historically used to prepackage the roles, such as team principle, you envisage users using in your application. In contrast, Permission Sets can be used to express the functionality in your application such as the ability to assign a driver to a race.

By now, most people are familiar with configuring profiles to control security. The issue is that they can eventually lead to a proliferation of profiles; as small differences in needs between users arise, profiles are then cloned and thus increases the administration overhead when performing common changes. To manage this, Salesforce created Permission Sets, which is a much more granular way of expressing security controls.

From an application perspective, you can package Permission Sets and even have them upgraded automatically as they change in accordance with the new or updated functionality in your application. You can also assign them after installation, in contrast to profiles, which can only be done during the installation. Also unlike profiles, administrators cannot edit them though they can clone them if there is a need (this is not possible in Professional and Group Editions of Salesforce). Because of these benefits, it is recommended that Permission Sets be used over profiles when packaging security information about your application.