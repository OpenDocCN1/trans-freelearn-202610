# # Enabling ASP.NET Core with IIS on Nano Server

In this chapter, we will cover the following recipes:

+   - Understanding ASP.NET Core with IIS on Nano Server
+   - Configuring ASP.NET Core with IIS on Nano Server
+   - Creating an IIS 10.0 virtual directory on Nano Server
+   - Configuring a virtual directory in IIS 10.0
+   - Uploading and testing web pages in a virtual directory

# ## Introduction

ASP.NET Core is an open source and cross-platform framework that runs on any operating system with or without an ASP.NET application pool (framework). It helps us develop and publish web-based applications independent of platforms such as macOS, Linux, or Windows Server Core. ASP.NET Core is also known as ASP.NET 5. Nano Server 2016 targets ASP.NET Core.

Once we start developing an ASP.NET Core application and publish it, it will automatically copy the all the required runtime DLLs and other files to the web application publish directory so that the core web application runs independently without requiring any framework on IIS Server.

ASP.NET 2.0 will not run on Nano.