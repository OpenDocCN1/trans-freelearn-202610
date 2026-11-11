# 6

# Authentication Controls and Membership Providers

The Membership Provider Model is one of the new key concepts introduced with ASP.NET 2.0, which makes it significantly easier to develop web applications that utilize third-party or custom membership stores. In addition, ASP.NET 2.0 ships with a number of authentication controls related to role membership, which vastly reduce the amount of code required to implement Forms Authentication and associated functionality in your applications.

MCMS has its own role-based authorization and user management system that cannot be extended. However the Membership Provider Model and the shipped controls can be used within MCMS applications to improve the implementation of Forms Authentication and provide a more elegant solution for ‘account mapping’ scenarios whereby authentication takes place against an external store and the accounts are mapped to Windows accounts for the purposes of MCMS authorization.

In this chapter we will first briefly look at the ASP.NET 2.0 authentication controls, before proceeding to implement an MCMS Membership Provider that supports the use of the authentication controls within MCMS applications.

# Authentication Controls

ASP.NET 2.0 ships with seven authentication-related composite controls such as the Login control (which provides a standard user interface for implementing Forms Authentication) and the PasswordRecovery control (which allows retrieval of a user’s password). These controls can be seen within the Login tab of the Visual Studio 2005 toolbox.