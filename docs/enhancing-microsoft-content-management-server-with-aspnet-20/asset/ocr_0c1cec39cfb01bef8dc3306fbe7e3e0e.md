> When developing on Windows Server 2003, the Virtual Website root must run in the same worker process (i.e. Application Pool) as all MCMS applications so that the MCMS ISAPI Filter can work as expected. This filter cannot route requests across worker-process boundaries. In effect this means that all MCMS applications will share the same ASP.NET version, as ASP.NET does not support side-by-side execution of different versions inside the same worker process. This is not necessary with IIS on Windows XP as it does not use Worker Process Isolation mode.

Next, we will check the authentication settings. For now, we will configure the website to use integrated Windows authentication. Only users with a domain or local user account will have access to the site. Later in Chapter 6 we will show alternative authentication methods such as Forms Authentication.

+   1. Click the **Directory Security** tab followed by the **Edit...** button, and note that the permissions are correctly inherited from the Virtual Web Site settings. In this example, we will use integrated Windows authentication.

![](bbox=[0.33, 0.48, 0.67, 0.77])