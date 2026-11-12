+   4. From Server Manager, select Tools | Active Directory Users, and Computers:

![](bbox=[0.21, 0.14, 0.785, 0.285])

Figure 4.13 – Server Manager tools

+   5. Please navigate to a user whose UPN suffix you wish to change. Right-click and select Properties.

+   6. Select the Account tab, and from the UPN suffix field dropdown, select the UPN suffix to be used. In this case, we will select the domain suffix, @milesbettersolutions.com, which we added in a previous step, using the Azure Directory Domains and Trusts console to replace the non-routable domain of @milesbettersolutions.local with the user's new UPN and their sign-in address:

![](bbox=[0.26, 0.48, 0.745, 0.66])

Figure 4.14 – UPN suffix selection

+   7. Repeat for each user (use a multi-select), close the Active Directory Users and Computers console, and prepare for the next task.

# Task 2 – Adding the custom domain name to Azure AD

In this task, we will add a custom domain to Azure AD and verify it to be used with Azure AD for directory synchronization. You will require a public domain that you own, and that has access to DNS records you can manage for this exercise:

+   1. Log in to the Azure portal with an account with a Global Administrator role for the tenant.