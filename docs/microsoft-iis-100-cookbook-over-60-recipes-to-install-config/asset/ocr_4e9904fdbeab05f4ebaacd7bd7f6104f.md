# Getting ready

To step through this recipe, you will need a running IIS 10.0 instance. You should also have administrative privileges.

# How to do it...

+   1. Open Server Manager on Windows Server 2016. Click on the Tools menu and open IIS Manager.
+   2. Expand the IIS server (WIN2016IIS). We get the listed Application Pools and Sites. You will get three listed websites--mysite.com, v2mysite.com, and v4mysite.com--which we've already created. Here, we will use only one.
+   3. Expand mysite.com under the left-hand side Sites panel:

![](bbox=[0.103, 0.405, 0.894, 0.79])

+   4. We are going to review the virtual directory now. We've already created a virtual directory named virtualdirectory inside the website mysite.com.