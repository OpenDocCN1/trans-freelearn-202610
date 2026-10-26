
7. Open 172.16.15.63\c$\inetpub\wwwroot\nanosite. As shown in this screenshot, the default.htm file has been created:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_226_0.png)

## How it works...

In this recipe, we uploaded default IIS website pages IISStart.htm and IISStart.png to the nanosite folder, which is located in the Nano Server C:\inetpub\wwwroot\nanosite folder. We created the default.htm file through PowerShell and verified that the file was created in the folder.

## Testing uploaded web pages

In this recipe, we will open the firewall port for Nano Server. We've already modified the port 8081 binding information for nanosite.com. We need to open the firewall port 8081 in Nano Server, which will allow us to access the website with port 8081. We will open the default.htm page, which we created through PowerShell.

6. You can see in the screenshot that the firewall Allow rule has been created. Now let's test nanosite.com:8081 to check whether it's working or not.

## PowerShell DSC:

PowerShell DSC is a platform for testing and ensuring the declarative state of a system. DSC allows you to scale complex deployments across environments, enables collaboration of management, and corrects for configuration drift.

## PowerShell ISE:

PowerShell Integrated Scripting Environment (ISE) is a Windows application that supports enhanced usage of PowerShell for beginners and experts alike. ISE's many features include:

- A built-in editor for writing, testing, and debugging scripts
- Full IntelliSense tab completion, syntax highlighting, and context-sensitive help
- A myriad of keyboard shortcuts
- Support for right-to-left languages
- Extensible add-ons

For more information, you can refer to https://msdn.microsoft.com/en-us/powershell/mt173057.aspx.

## IIS administration with PowerShell cmdlets

In this recipe, we will log in to the different PowerShell remote-management modules for Nano Server to manage IIS Server. We will log in through Server Manager to PowerShell, and we will also review the PowerShell remote script.

## Getting ready

In this recipe, we are going to access Nano Server through PowerShell and the IISAdministrator module. You will need a Windows Server 2016 virtual or physical instance for remote PowerShell management of Nano Server 2016. For all this installation and configuration work, you should have administrative privileges.

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_234_0.png)

5. In the **All Servers** window, we have added the IP 172.16.15.63, which is our Nano Server 2016 IP. Now right-click on the Nano Server IP, `172.16.15.63`, and click on **Manage As...**. It will open a new authentication window called **Windows Security for Nano Server**, as shown here:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_234_1.png)

14. Go to the **WIN2016IIS** server to connect to Nano Server 2016 through PowerShell. You have to first log in to the WIN2016IIS server (any Windows Server 2016 or Windows 10 machine will do). We will use PowerShell to remotely connect to Nano Server at 172.16.15.63.

15. Now press Windows + R. You will get the Run window. Open PowerShell ISE with administrative rights, type the command RunAs /user:Administrator PowerShell_ISE.exe, and press OK. Now it will ask you the administrator password for your Windows Server 2016 instance. Type the administrator password and press Enter. Now you should get the PowerShell window, as shown here:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_237_0.png)

16. We performed this step earlier in Chapter 7, Deploying IIS 10.0 on Nano Server. Here we just have to recap. You can see that we have several easy options available in PowerShell ISE. Using the Script Editor, we can write our script and it will also show you the advanced automation writing option. You can save and use it several times. We have Commands, which displays available commands in PowerShell and available PowerShell module features. You can open the simple PowerShell prompt, which is available in Windows Server; if you need administrative privileges with PowerShell ISE, you'll need the administrator username and password to access PowerShell ISE. Once you are accessing Nano Server remotely, make Nano Server a trusted host, and then get connected to it with the proper username and password.

## How it works...

In this recipe, we opened Server Manager on the WIN2016IIS server and added the Nano Server host to the All Servers list. We then authenticated Nano Server with Server Manager and opened PowerShell remote management for Nano Server to manage IIS Server. We logged in through Server Manager to PowerShell, imported the IISAdministration module, and ran the Get-Site command. Also, we reviewed the PowerShell ISE remote script.

## Creating an advanced IIS 10.0 website on Nano Server

In this recipe, we will create a website with a website header (website name) and also add an application pool. We will create a separate application pool and add the application pool to our website. We will find all the available listed application pools in IIS Server.

## Getting ready

We are going to remotely connect Nano Server through PowerShell and manage IIS over the IISAdministrator module. You will need a Windows Server 2016 virtual or physical server for remote PowerShell management of Nano Server 2016. For all this installation and configuration work, you should have administrative privileges.

## How to do it...

- 1. Log in to Windows Server 2016. The IP address of Windows Server 2016 is 172.16.15.60 and the name of the server is **WIN2016IIS**.
- 2. Press Windows + R on your keyboard. You will get the **Run** window. Open PowerShell ISE with administrative rights, type `RunAs /user:Administrator PowerShell_ISE.exe` in the **Run** window, and press **OK**.
- 3. Now it will ask you the administrative password for you Windows Server 2016 instance (not Nano Server). Type the administrator password and press *Enter*. The PowerShell window will open.
- 4. Now we have to remote-connect to Nano Server 2016 on the IP 172.16.15.63.
- 5. We will use the following command from PowerShell:

```
Enter-PSSession -ComputerName "172.16.15.63" -Credential \Administrator
```

You can use either the computer name or IP address for `-ComputerName`. We are using the IP address in this command.

- 6. Now you will get a pop-up window for the Nano Server administrator username and password. We will enter the Nano server administrator password and press **OK** to log in. You have connected successfully. Run the `Import-module iisAdministration` command. Press **Enter** and you will get the PowerShell module imported.
- 7. Let's create the website with an advanced script. We will use the following command:

```
$manager = Get-IISServerManager
$site = $manager.Sites.Add("v5mysite", "http", "*:8082:v5mysite.com", "C:\v5mysite") $site.Id = 4
$site.Bindings.Add("*:8083:", "http")$manager.CommitChanges()
```

10. Now let's check the created website, v5mysite. Run the command `Get-IISSite` in PowerShell:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_241_0.png)

11. You'll see in the figure that it shows that you have a website called **v5mysite**, ID 4, status `Started`, and physical path `c:\v5mysite`, and we have two pieces of binding information, one with the header and the other without.

12. Let's create the application pool in IIS 10.0 on Nano Server. Run the following command in PowerShell:

```
$manager = Get-IISServerManager
$pool = $manager.ApplicationPools.Add("NanoAPP")
$pool.ManagedPipelineMode = "Integrated"
$pool.ManagedRuntimeVersion = "v4.0"
$pool.Enable32BitAppOnWin64 = $false
$pool.AutoStart = $true
$pool.StartMode = "OnDemand"
$pool.ProcessModel.IdentityType = "ApplicationPoolIdentity"
$manager.CommitChanges()
```
