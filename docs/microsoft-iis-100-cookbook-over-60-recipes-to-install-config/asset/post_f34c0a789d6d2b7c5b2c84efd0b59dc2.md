
12. Type the dism/online/addpackage/packagepath:c:\packages\Microsoft-OneCore-ReverseForwarders-Package.cab and dism /online /add-package /packagepath:c:\packages\en-us\Microsoft-OneCore-ReverseForwarders-Package.cab DISM commands and run them, as shown here:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_272_0.png)

13. Let's move on to configuring ASP.NET Core. We have already copied the required DLL and XML files. Type the following commands in PowerShell:

```
$sm = Get-IISServerManager
$sm.GetApplicationHostConfiguration()
.RootSectionGroup.Sections.Add("appSettings")
$appHostconfig =
$sm.GetApplicationHostConfiguration()
$section =
```

## Enabling ASP.NET Core with IIS on Nano Server

```
```powershell
$appHostConfig.GetSection("system.webServer/handlers")
$section.OverrideMode="Allow"
$sectionHttpPlatform = $appHostConfig.RootSectionGroup.SectionGroups["system.webServer"].Sections.Add("httpPlatform")
$sectionHttpPlatform.OverrideModeDefault = "Allow"
$globalModules = Get-IISConfigSection "system.webServer/globalModules" | Get-IISConfigCollection
New-IISConfigCollectionElement $globalModules -ConfigAttribute @{"name"="httpPlatformHandler"; "image"="%SystemRoot%\\system32\\inetsrv\\httpPlatformHandler.dll"}
$modules = Get-IISConfigSection "system.webServer/modules" | Get-IISConfigCollection
New-IISConfigCollectionElement $modules -ConfigAttribute @{"name"="httpPlatformHandler"}
$sm.CommitChanges()
```
```

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_273_0.png)

14. You can see in the screenshot that we've configured the required steps to run ASP.NET Core on Nano Server. However, you have to do a few more things in your core application and publish the code with the DNU publish command. You can find out more at `https://blogs.iis.net/davidso/nano/aspnet`.

## How it works...

In this recipe, we installed HttpPlatformHandler on the WIN2016IIS server (physical server, not Nano Server). We then copied `httpPlatformHandler.dll` and `httpplatform_schema.xml` from WIN2016IIS and pasted them to Nano Server. We configured `httpPlatformHandler.dll` and `httpplatform_schema.xml` on Nano Server through PowerShell. We also configured the ReverseForwarders package on Nano Server.

## Creating an IIS 10.0 virtual directory on Nano Server

In this recipe, we will create a website named `NanoAspnet`. We will create a virtual directory inside the website called `NanoAspnet`. We will then remotely connect to Nano Server using PowerShell and manage IIS using the `IISAdministration` module and Nano Server.

## Getting ready

To step through this recipe, you will need a Windows Server 2016 virtual or physical server for remote PowerShell management and creating the virtual directory on Nano Server 2016. For all this installation and configuration work, you should have administrative privileges.

## How to do it...

- 1. Log in to Windows Server 2016. The IP is `172.16.15.60` and the name of the server is **WIN2016IIS**. Press *Windows* + R. You will get the **Run** window. Open PowerShell ISE with administrative rights, type `RunAs /user:Administrator PowerShell_ISE.exe`, and press **OK**.

6. You can see from the screenshot that we have created the website NanoAspNet with port 8000. Now let's create the virtual directory MyApp on NanoAspNet. First, we have to create a physical directory (folder on Nano Server) that we can use for the virtual directory. We create a directory called MyApp under `C:\inetpub\wwwroot`, as shown here:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_276_0.png)

## Enabling ASP.NET Core with IIS on Nano Server

7. Now open PowerShell, which is already connected with our Nano Server instance, and type the following command to create a virtual directory:

```
```powershell
$manager = Get-IISServerManager
$app = $manager.Sites["NanoAspNet"].Applications.Add("/MyApp", "c:\inetpub\wwwroot\MyApp")
$manager.CommitChanges()
```
```

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_277_0.png)

8. You have successfully created the MyApp virtual directory for NanoAspNet.

## How it works...

In this recipe, we remotely connected PowerShell with Nano Server to manage IIS over the IISAdministrator module. We used a Windows Server 2016 virtual/physical server instance for remote PowerShell management and creating a virtual directory on Nano Server 2016. We also created a website named `NanoAspnet` and created a virtual directory `MyApp` inside `NanoAspNet`.

## Configuring a virtual directory in IIS 10.0

In this recipe, we will create an application pool and assign it to our virtual directory, `MyApp`. We will create a firewall rule to allow inbound port `8000` for `NanoAspNet`.

## Getting ready

To step through this recipe, you will need a Windows Server 2016 virtual or physical server for remote PowerShell management and configuring a virtual directory on Nano Server 2016. For all this installation and configuration, you should have administrative privileges.

## How to do it...

- 1. Log in to Windows Server 2016. The IP is `172.16.15.60` and the name **WIN2016IIS**. Press *Windows + R* key. You will get the **Run** window. Open PowerShell ISE with administrative rights: type `RunAs /user:Administrator PowerShell_ISE.exe` and press **OK**.
- 2. Now, it will ask you the administrator password for your Windows Server 2016 (not Nano Server) machine. Type the password and press *Enter*. You will get the PowerShell window.
- 3. Now you need to remotely connect to Nano Server 2016 using the IP `172.16.15.63`. Write the command `Enter-PSSession -ComputerName "172.16.15.63" -Credential ~\Administrator.` You will get a popup window for the Nano Server administrator account password. Enter the password and press **OK**. You'll have connected successfully. Now you have to write `Import-module iisAdministration` and press *Enter*. This will import the PowerShell module.

4. Type the command `$sm = Get-IISServerManager\n$sm.ApplicationPools.Add("NanoAppPool")` to create the application pool NanoAppPool, as shown here:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_279_0.png)

5. We have now created the NanoAppPool application pool. Let's move to assigning NanoAppPool to the MyApp virtual directory. Type the following command:

```
$manager = Get-IISServerManager
$website = $manager.Sites["NanoAspNet"]
$website.Applications["/MyApp"].ApplicationPoolName = "NanoAppPool"
$manager.CommitChanges()
```

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_280_0.png)

6. Let's check whether this command was successful. Access Nano Server remotely at \\172.16.15.63\c$\Windows\System32\inetsrv\config. Inside the config folder, you will get the ApplicationHost.config file. Open ApplicationHost.config and search the MyApp virtual directory, as shown here:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_281_0.png)

7. Here you can see NanoAspNet website with a virtual directory MyApp and virtual directory application pool NanoAppPool.

8. We have to open the firewall port 8000 (by default, Nano Server allows IIS ports 80 and 443). Type the command New-NetFirewallRule -DisplayName "Allow Inbound Port 8000" -Direction Inbound -LocalPort 8000 -Protocol TCP -Action Allow, as shown here:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_282_0.png)

9. As you can see, the firewall allows port 8000, and the rule has been created.

## How it works...

In this recipe, we created the application pool NanoAppPool and assigned the application pool to MyApp. We checked the MyApp application pool, NanoAppPool, to the ApplicationHost.config file. We also created a firewall rule to allow inbound port 8000 for website NanoAspNet.

## Uploading and testing web pages in a virtual directory

In this recipe, we will log in to Nano Server remotely. We will use PowerShell and check our created sites. We will access the Nano Server C$ share to upload our web pages. We will test the NanoAspNet website on port 8000 and also test the MyApp virtual directory.

## Getting ready

To step through this recipe, you will need a Windows Server 2016 virtual or physical server for remote PowerShell management and configuring virtual directory on Nano Server 2016. For all this installation and configuration work, you should have administrative privileges.

## How do to it...

- 1. Log in to Windows Server 2016. The IP is 172.16.15.60 and the name of the server is WIN2016IIS. Press Windows + R. You will get the Run window. We have to open PowerShell ISE with administrative rights. Type RunAs /user:Administrator PowerShell_ISE.exe in the Run window and press OK.
- 2. Now it will ask you the administrator password for your Windows Server 2016 (not Nano Server) machine. Type the password and press Enter. You will get the PowerShell window.
- 3. Now you have to remotely connect to Nano Server 2016 using the IP 172.16.15.63. Run the following command in PowerShell:
- ```
+ "Enter-PSSession -ComputerName "172.16.15.63" -Credential ~\Administrator"
+ ```
- 4. Now you will get a popup window for the Nano Server administrator account password. Enter the password and press OK to log in. You'll have connected successfully.
