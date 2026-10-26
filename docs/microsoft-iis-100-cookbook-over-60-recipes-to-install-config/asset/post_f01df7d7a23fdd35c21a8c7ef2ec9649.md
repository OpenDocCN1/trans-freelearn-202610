
13. Click on **Yes**. Now you have to create a new entry for `v5mysite.htm` in the **Default Document** settings. Type the following command in PowerShell:

```
Get-IISConfigSection -SectionPath "system.webServer/defaultDocument" | Get-IISConfigCollection -CollectionName "files" | New-IISConfigCollectionElement -ConfigAttribute @{Value = "v5mysite.htm"} –AddAt 0
```

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_260_0.png)

14. Let's test the website `v5mysite.com` and check the default page. Open Internet Explorer and type the URL `http://172.16.15.63:8082/`:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_261_0.png)

15. Here you can see that the website opens the default page, `v5mysite.htm`.

## How it works...

In this recipe, we tested the website `v5mysite.com`, which we created and configured. We created an Allow rule on Nano Server for port `8082` in the firewall. We set up the **Default Document** settings for the default page `v5mysite.htm` of website `v5mysite.com`, and we made the default page entry for `v5mysite.htm` at the top. We browsed the website `v5mysite.com` and saw the default page.

## Enabling ASP.NET Core with IIS on Nano Server

In this chapter, we will cover the following recipes:

- Understanding ASP.NET Core with IIS on Nano Server
- Configuring ASP.NET Core with IIS on Nano Server
- Creating an IIS 10.0 virtual directory on Nano Server
- Configuring a virtual directory in IIS 10.0
- Uploading and testing web pages in a virtual directory

### Introduction

ASP.NET Core is an open source and cross-platform framework that runs on any operating system with or without an ASP.NET application pool. It helps us develop and publish web-based applications independent of platforms such as macOS, Linux, or Windows Server Core. ASP.NET Core is also known as ASP.NET 5. Nano Server 2016 targets ASP.NET Core.

Once we start developing an ASP.NET Core application and publish it, it will automatically copy all the required runtime DLLs and other files to the web application publish directory so that the core web application runs independently without requiring any framework on IIS Server.

ASP.NET 2.0 will not run on Nano.

## Understanding ASP.NET Core with IIS on Nano Server

In this recipe, we will create an ASP.NET Core website application and run it on Visual Studio 2017.

### Getting ready

To step through this recipe, you need Visual Studio 2017 set up. For all the installation and configuration work, you should have administrative privileges.

### How to do it...

1. Log in to your Windows 10 PC or Windows Server 2016. We've already installed Visual Studio for testing, so open it.
2. Open the File menu, create a new project, and select **.Net Core** and application type **ASP.NET Core Web Application (.NET Core)**, as shown here:

### New Project

.NET Framework 4.5.2
Sort by: Default
Search Installed Templates: (Ctrl+E)
Recent
Installed
Templates
Visual C#
Windows Universal
Windows Classic Desktop
Web
.NET Core
.NET Standard
Android
Cloud
Cross-Platform
iOS
Test
tvOS
WCF
Visual Basic
Visual C++
Online
Console App (.NET Core) Visual C#
Class Library (.NET Core) Visual C#
Unit Test Project (.NET Core) Visual C#
xUnit Test Project (.NET Core) Visual C#
ASP.NET Core Web Application (.NET Core) Visual C#
Type: Visual C#
Project templates for creating ASP.NET Core applications for Windows, Linux and macOS using .NET Core.
Name: WebApplication4
Location: c:\users\administrator\documents\visual studio 2017\Projects
Solutions: Create new solution
Solution name: WebApplication4
Create directory for solution
Create new Git repository
OK
Cancel

3. Click on the OK button. You will get a New ASP.NET Core window to select the type of ASP.NET Core application, as shown here:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_265_0.png)

4. Now select the Web Application option and click on OK. You'll have the default ASP.NET web application created. This is just for demonstration to show you how an ASP.NET web application looks.

5. Now let's run the created project on Visual Studio 2017, and you will get the web application page, as shown in the following screenshot.

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_265_1.png)

To run an application from Visual Studio, click on the "play" sign in the toolbar at the top or hit F5.

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_266_0.png)

6. You can now see the default sample application, which we created with ASP.NET core.

### How it works...

In this recipe, we logged in to Windows Server 2016 and opened the Visual Studio 2017 software. We also created the ASP.NET Core website application and ran it on Visual Studio 2017.

## Configuring ASP.NET Core with IIS on Nano Server

In this recipe, we will install `HttpPlatformHandler` on the **WIN2016IIS** server and configure `httpPlatformHandler.dll` and `httpplatform_schema.xml` from **WIN2016IIS** to Nano Server. We will configure our ASP.NET Core application on Nano Server. We will connect Nano Server remotely through PowerShell and manage IIS over the `IISAdministrator` module and Nano Server.

2. Click on the selected link, and it will start downloading **HttpPlatformHandler**.
3. Run **HttpPlatformHandler** on WIN2016IIS. You will get the installation page of **HttpPlatformHandler**, as shown here:
4. Click on **Next**, follow the default steps, and finish the installation of **HttpPlatformHandler**.

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_268_0.png)

5. On the WIN2016IIS server, go to C:\Windows\System32\inetsrv, find httpPlatformHandler.dll, and copy it to the desktop of the WIN2016IIS server. I have created a folder named ASP Net core\ASP.net, and I will paste httpPlatformHandler.dll in it. Now, go to C:\Windows\System32\inetsrv\config\schema and copy httpplatform_schema.xml to Asp Net core\ASP.net, as shown here:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_269_0.png)

6. As shown in the screenshot, we have copied the httpplatform_schema.xml and httpPlatformHandler.dll files.

7. Copy httpPlatformHandler.dll from ASP Net Core\ASP.net, remotely access the Nano Server location \\172.16.15.63\c$\Windows\System32\inetsrv\, and paste it, as shown here:

## Enabling ASP.NET Core with IIS on Nano Server

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_270_0.png)

8. Copy httpplatform_schema.xml from ASP Net Core\asp.net, remotely access the Nano Server location \\172.16.15.63\c$\Windows\System32\inetsrv\config\schema, and paste it.

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_270_1.png)

8. Log in to Windows Server 2016. The IP is `172.16.15.60` and the name of the server is **WIN2016IIS**. Press *Windows + R* on your keyboard, and you will get the *Run* window. Open PowerShell ISE with administrative rights, type in the **Run** command window `RunAs /user:Administrator PowerShell_ISE.exe`, and press **OK**.

> In this command, `Administrator` is the username for that machine; you can use any username that has administrator rights to perform the given task.

9. Now it will ask you the administrator password for your Windows Server 2016 instance. Type the administrator password and press *Enter*. You will get the PowerShell window.

10. Now you have to remotely connect to Nano Server 2016 at `172.16.15.63`. Run the command `Enter-PSSession -ComputerName "172.16.15.63" -Credential ~\Administrator`. Now you will get a popup window for the Nano Server administrator account password. Enter the password and press **OK** to log in. You have now connected successfully. Write `Import-module isAdministrator` and press *Enter*. This will import the PowerShell module.

11. Now you need to install the Reverse Forwarders packages, which you created in the `Packages` folder in the Nano Server `C:\` drive, and copy the Nano Server Packages, as shown here:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_271_0.png)
